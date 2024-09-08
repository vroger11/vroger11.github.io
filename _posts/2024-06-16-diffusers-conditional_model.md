---
layout: post
comments: true
title:  "Diffusers - generate images from labels with conditional model"
ref: diffusers-tutorial-part2
date:   2024-06-16 08:00:00 +0200
categories: blog deeplearning python
category: blog
lang: en
excerpt: My second post on the Diffusers library. Here, we will generate images using a conditioned model using one-hot labels.
---

Following my previous post on unconditional generation using the diffusers library (link [here](/blog/deeplearning/python/2024/06/02/diffusers-unconditional_model)), we will dive into conditional generation using labels. While many tutorials and examples focus on text-guided diffusion models, in this blog post, we will explore the use of one-hot labels to condition our diffusion model. This approach can be particularly useful in scenarios where we want to generate images based on discrete categories rather than textual descriptions.

## Conditional diffusion

Conditional diffusion models extend unconditional diffusion by introducing additional information during the generation process. This additional information, or conditioning input, can guide the model to produce samples that belong to a specific category or possess certain characteristics. Common conditioning inputs include text descriptions, class labels, or other image attributes.

In this tutorial, we will use [one-hot](https://en.wikipedia.org/wiki/One-hot) encoded labels as the conditioning input. One-hot encoding is a representation of categorical variables as binary vectors. This method is especially suitable for tasks like image generation where each image corresponds to a discrete category. Taken back to our previous dataset, we will have 5 categories : `[hero, non-hero, food, spell, side-facing hero]`.

## Define the unet model, the noise scheduler and network to embed our labels

Compared to last time, we will use the `UNet2DConditionModel` class for our model, and we will pass the ground truth as embeddings.

```python
from diffusers import DDPMScheduler
from diffusers.models import UNet2DConditionModel

class_emb_size = 64

# Define the UNet model
unet = UNet2DConditionModel(
    encoder_hid_dim=class_emb_size,
    sample_size=(16, 16),                                       # Input image size
    in_channels=3,                                              # Number of input channels (e.g., 3 for RGB)
    out_channels=3,                                             # Number of output channels
    layers_per_block=2,                                         # Layers per block in the UNet
    block_out_channels=(64, 128),                               # Channels in each block
    down_block_types=("DownBlock2D", "CrossAttnDownBlock2D"),   # Types of down blocks
    up_block_types=("CrossAttnUpBlock2D", "UpBlock2D"),         # Types of up blocks
)

# Define the DDPM scheduler
noise_scheduler = DDPMScheduler(num_train_timesteps=200)
```

To introduce conditions with such a model, you can use cross attention block, hence the use of `"CrossAttnDownBlock2D"` and `"CrossAttnUpBlock2D"` in our unet.
Now we need some network to embed our one-hot data into the desired embedding size, here we will take the similar network as in the deeplearning.ai course:

```python
class UnsqueezeLayer(nn.Module):
    """Generic layer to unsqueeze its input."""

    def __init__(self, dim: int) -> None:
        super(UnsqueezeLayer, self).__init__()
        self.dim = dim

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        return torch.unsqueeze(x, dim=self.dim)


# We have to create this custom class to be able to use our sequential model inside our pipeline.
class CustomSequential(nn.Sequential):
    """Extend sequential to add `device` and `dtype` properties.

    It supposes that all parameters shares the same device and uses the same dtype.
    """

    @property
    def device(self):
        return next(self.parameters()).device

    @property
    def dtype(self):
        return next(self.parameters()).dtype

num_classes = 5

emb_net = CustomSequential(
    nn.Linear(num_classes, class_emb_size),
    nn.GELU(),
    nn.Linear(class_emb_size, class_emb_size),
    UnsqueezeLayer(dim=1),
)
```

Here, we used a custom class to have `device` and `dtype` properties on this model (to be able to use it in our final pipeline). As `UNet2DConditionModel` is designed to have text embeddings of shape `[batch, sequence_length, feature_dim]`, our `emb_net` finish with a squeeze to have the embedding of shape `[batch_size, 1, class_emb_size]`. Having no sequence in our labels, this suits better our case.

## Train the model

Here I modified our train function to add mixed precision (to speed up the training) and our conditioning:

```python
def train(
    unet: UNet2DConditionModel,
    emb_net: nn.Module,
    noise_scheduler: DDPMScheduler,
    dataloader: DataLoader,
    num_epochs: int,
    lr: float,
):
    epochs = range(num_epochs)

    optimizer = Adam(chain(unet.parameters(), emb_net.parameters()), lr=lr)
    scaler = GradScaler("cuda" if torch.cuda.is_available() else "cpu")  # For mixed precision
    unet.train()

    for epoch in epochs:
        epoch_loss = 0

        for batch in tqdm(dataloader):
            optimizer.zero_grad()

            # Assuming your dataloader provides images and associated target
            images, labels = batch
            images = images.to(unet.device)
            labels = labels.to(dtype=torch.float32, device=unet.device)

            with autocast("cuda" if torch.cuda.is_available() else "cpu"):  # Mixed precision
                # Generate random noise
                noise = torch.randn(images.shape, device=unet.device)

                # Generate random timesteps and apply the noise scheduler
                timesteps = torch.randint(
                    0,
                    noise_scheduler.config.num_train_timesteps,
                    (images.shape[0],),
                    device=unet.device,
                )

                noisy_images = noise_scheduler.add_noise(images, noise, timesteps)

                # Compute the class embeddings
                enc_labels = emb_net(labels)

                # Forward pass through the model with labels embeddings
                predicted_noise = unet(
                    noisy_images, timesteps, enc_labels, class_labels=labels
                ).sample

                # Compute loss (mean squared error between actual and predicted noise)
                loss = torch.nn.functional.mse_loss(predicted_noise, noise)

            # Backward pass and optimization
            scaler.scale(loss).backward()
            scaler.step(optimizer)
            scaler.update()

            epoch_loss += loss.item()

        epoch_loss = epoch_loss / len(dataloader)
        print(f"Epoch {epoch + 1}/{num_epochs}, Loss: {epoch_loss}")
```

It prints the mean loss of each epoch. To train the model, we forward the model on random timesteps with our data and embeddings. Then, we compute the loss on the predicted noise to optimize the parameters of our `unet` and `emb_net`. In the notebook we kept the dataloader from deeplearning.ai so nothing new under the sun. As you can see, our training loop is not that much different from the training loop of our unconditional unet.

**Note:** I did not randomly mask out the labels (like in the deeplearning.ai tutorial) as it did not improve the results.

Now we can save our pretrained `unet` and `emb_net`.

```python
unet.save_pretrained(pre_trained_unet_path)
torch.save(emb_net.state_dict(), pre_trained_emb_net_path)
```

## Do inferences with the trained unet

Now we learned our model, we can load it and use it on a pipeline from the diffusers library.
First, let's load our models:

```python
unet = UNet2DConditionModel.from_pretrained(pre_trained_unet)
emb_net.load_state_dict(torch.load(pre_trained_emb_net))
```

Pretty easy! Now the hard part, we need to create a custom pipeline to use our models. Here, we will not start from zero by extending the `DDPMPipeline`:

```python
from diffusers import DDPMPipeline


class ConditionalDDPMPipeline(DDPMPipeline):
    def __init__(
        self, unet: UNet2DConditionModel, class_net: CustomSequential, scheduler: DDPMScheduler
    ) -> None:
        super().__init__(unet=unet, scheduler=scheduler)
        self.class_net = class_net
        # to let the pipeline change the model device and/or type
        self.register_modules(class_net=class_net)

    @torch.no_grad()
    def __call__(
        self,
        class_label: list[list[float]],
        generator: Optional[Union[torch.Generator, List[torch.Generator]]] = None,
        num_inference_steps: int = 1000,
        output_type: Optional[str] = "pil",
        return_dict: bool = True,
    ) -> Union[ImagePipelineOutput, Tuple]:
        r"""
        The call function to the pipeline for generation.

        Args:
            class_label (list[list[float]]):
                list of one-hot examples. len(class_label) represents the number of examples to generate.
            generator (`torch.Generator`, *optional*):
                A [`torch.Generator`](https://pytorch.org/docs/stable/generated/torch.Generator.html) to make
                generation deterministic.
            num_inference_steps (`int`, *optional*, defaults to 1000):
                The number of denoising steps. More denoising steps usually lead to a higher quality image at the
                expense of slower inference.
            output_type (`str`, *optional*, defaults to `"pil"`):
                The output format of the generated image. Choose between `PIL.Image` or `np.array`.
            return_dict (`bool`, *optional*, defaults to `True`):
                Whether or not to return a [`~pipelines.ImagePipelineOutput`] instead of a plain tuple.

        Returns:
            [`~pipelines.ImagePipelineOutput`] or `tuple`:
                If `return_dict` is `True`, [`~pipelines.ImagePipelineOutput`] is returned, otherwise a `tuple` is
                returned where the first element is a list with the generated images
        """
        batch_size = len(class_label)
        # Sample gaussian noise to begin loop
        if isinstance(self.unet.config.sample_size, int):
            image_shape = (
                batch_size,
                self.unet.config.in_channels,
                self.unet.config.sample_size,
                self.unet.config.sample_size,
            )
        else:
            image_shape = (batch_size, self.unet.config.in_channels, *self.unet.config.sample_size)

        if self.device.type == "mps":
            # randn does not work reproducibly on mps
            image = randn_tensor(image_shape, generator=generator)
            image = image.to(self.device)
        else:
            image = randn_tensor(image_shape, generator=generator, device=self.device)

        labels = torch.tensor(class_label, device=self.device)
        enc_labels = self.class_net(labels)

        # set step values
        self.scheduler.set_timesteps(num_inference_steps)

        for t in self.progress_bar(self.scheduler.timesteps):
            # 1. predict noise model_output
            model_output = self.unet(image, t, enc_labels, class_labels=labels, return_dict=False)[
                0
            ]

            # 2. compute previous image: x_t -> x_t-1
            image = self.scheduler.step(model_output, t, image, generator=generator).prev_sample

        image = (image / 2 + 0.5).clamp(0, 1)
        image = image.cpu().permute(0, 2, 3, 1).numpy()
        if output_type == "pil":
            image = self.numpy_to_pil(image)

        if not return_dict:
            return (image,)

        return ImagePipelineOutput(images=image)
```

Basically, it is the same code as the `DDPMPipeline` from diffusers except we have our `emb_net` added to it to do generations conditioned with labels.
Now let's declare our pipeline with cuda if available, generate some samples and visualize them:

```python
pipeline = ConditionalDDPMPipeline(unet=unet, class_net=class_net, scheduler=noise_scheduler)
pipeline.to("cuda" if torch.cuda.is_available() else "cpu")

generated_image = pipeline(
    [
        # hero, non-hero, food, spell, side-facing hero
        [1.0, 0.0, 0.0, 0.0, 0.0],
        [1.0, 0.0, 0.0, 0.0, 0.0],
        [1.0, 0.0, 0.0, 0.0, 0.0],
        [1.0, 0.0, 0.0, 0.0, 0.0],
        [0.0, 1.0, 0.0, 0.0, 0.0],
        [0.0, 1.0, 0.0, 0.0, 0.0],
        [0.0, 1.0, 0.0, 0.0, 0.0],
        [0.0, 1.0, 0.0, 0.0, 0.0],
        [0.0, 0.0, 1.0, 0.0, 0.0],
        [0.0, 0.0, 1.0, 0.0, 0.0],
        [0.0, 0.0, 1.0, 0.0, 0.0],
        [0.0, 0.0, 1.0, 0.0, 0.0],
        [0.0, 0.0, 0.0, 1.0, 0.0],
        [0.0, 0.0, 0.0, 1.0, 0.0],
        [0.0, 0.0, 0.0, 1.0, 0.0],
        [0.0, 0.0, 0.0, 1.0, 0.0],
        [0.0, 0.0, 0.0, 0.0, 1.0],
        [0.0, 0.0, 0.0, 0.0, 1.0],
        [0.0, 0.0, 0.0, 0.0, 1.0],
        [0.0, 0.0, 0.0, 0.0, 1.0],
    ],
    num_inference_steps=200,
)

fig = plot_generated_images(generated_image.images, 5, 4)
fig.show()
```

![generated images](/assets/images/diffusers/tutorial_2_conditional.png)

Here we go, we have generated our first conditioned examples using the diffusers library with one-hot labels.
The full notebook and weights is available [here](https://github.com/vroger11/diffusers-tutorials).

I hope this helps and/or inspires some of you.

See you again, Vincent.
