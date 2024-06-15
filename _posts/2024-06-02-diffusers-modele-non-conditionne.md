---
layout: post
comments: true
title:  "Diffusers - générer des images avec un modèle non conditioné"
ref: diffusers-tutorial-part1
date:   2024-06-02 08:00:00 +0200
categories: blogue deeplearning python
category: blogue
lang: fr
---

Voici mon premier article sur l'utilisation de la bibliothèque [Diffusers](https://github.com/huggingface/diffusers).

Ces dernières années, les modèles de diffusion se sont imposés comme un outil puissant dans le domaine de la modélisation générative, rivalisant souvent avec les GAN (Generative Adversarial Networks) dans la production d'images de haute qualité (voir [this paper](https://arxiv.org/abs/2105.05233) de 2021 et [stable diffusion](https://github.com/CompVis/stable-diffusion?tab=readme-ov-file) de 2022). Ces modèles fonctionnent en simulant un processus de diffusion où les données sont progressivement bruitées puis débruitées pour générer de nouveaux échantillons.

De nouveaux cours apparaissent pour expliquer le fonctionnement de ces méthodes. L'un d'entre eux est [How Diffusion Models Works](https://www.deeplearning.ai/short-courses/how-diffusion-models-work/) de DeepLearning.ai. Il fournit d'excellents carnets de notes pour créer notre premier modèle de diffusion/pipeline. Cependant, il est fastidieux de mettre en œuvre une telle approche à partir de zéro en utilisant uniquement la bibliothèque PyTorch (mais c'est un excellent moyen d'apprendre le fonctionnement d'une telle méthode).

Dans cet article de blog, nous allons nous plonger dans l'implémentation d'un UNet inconditionnel en utilisant la bibliothèque [Diffusers](https://github.com/huggingface/diffusers) de deeplearning.ai. Nous allons parcourir les composants clés du code, expliquer comment le processus de diffusion est modélisé avec la bibliothèque Diffusers. Tout le code et un notebook de démonstrations peuvent être trouvés [ici](https://github.com/vroger11/diffusers-tutorial).

**Note:** Ce billet n'explique pas le fonctionnement des modèles de diffusion, mais comment utiliser la librairie diffusers pour se simplifier la vie.

## Pourquoi la bibliothèque diffusers ?

Cette bibliothèque est soutenue par Hugging Face et offre un cadre robuste et flexible pour travailler avec des modèles de diffusion. Voici les principales raisons pour lesquelles vous devriez envisager d'utiliser Diffusers pour vos projets :

- Facilité d'utilisation avec une API de haut niveau pour concevoir des modèles UNET.
- Documentation complète, même si les tutoriels et les exemples ne couvrent pas tous les cas.
- Communauté active et support, avec plus de 23k étoiles sur leur dépôt GitHub.

## Définir le modèle UNET et un planificateur de bruit

Avant d'entraîner un modèle, nous devons le définir et définir l'ordonnanceur de bruit associé :

```python
from diffusers import DDPMScheduler
from diffusers.models import UNet2DModel

# Définir le modèle UNet
# Note : ce modèle utilise moins de paramètres que le cours deeplearning.ai, car il n'est pas nécessaire d'avoir un modèle aussi grand pour cette tâche
model = UNet2DModel(
    sample_size=(16,16),                              # Taille de l'image d'entrée
    in_channels=3,                                    # Nombre de canaux d'entrée (par exemple, 3 pour RVB)
    out_channels=3,                                   # Nombre de canaux de sortie
    layers_per_block=2,                               # Couches par bloc dans l'UNet
    block_out_channels=(128, 64),                     # Canaux dans chaque bloc
    down_block_types=("DownBlock2D", "DownBlock2D"),  # Types de blocs descendants
    up_block_types=("UpBlock2D", "UpBlock2D")         # Types de blocks ascendants
)

# Définir le planificateur DDPM
noise_scheduler = DDPMScheduler(num_train_timesteps=500)
```

Et c'est tout ! Le [UNet2DModel](https://huggingface.co/docs/diffusers/api/models/unet2d) peut nous faire gagner énormément de temps dans la définition d'un tel modèle.

## Entraîner le modèle

Ici, j'ai fait une fonction simple de formation comprenant les bases pour ne pas submerger les lecteurs 😉 :

```python
def train(unet: UNet2DModel, noise_scheduler: DDPMScheduler, dataloader: DataLoader, num_epochs: int, lr: float) -> None:
    """Entraîne l'unet à partir de son noise_scheduler et d'un dataloader.

    Parameters
    ----------
    unet : UNet2DModel
        Le modèle unet à entraîner.
    noise_scheduler : DDPMScheduler
        Le planificateur de bruit à utiliser lors de l'entraînement.
    dataloader : DataLoader
        Le chargeur de données contenant les images à reproduire.
    num_epochs : int
        Le nombre d'époques pour entraîner l'unet.
    lr : float
        Le taux d'apprentissage à utiliser pour entraîner l'unet.
    """
    epochs = range(num_epochs)
    losses = np.zeros(num_epochs)

    optimizer = Adam(unet.parameters(), lr=lr)
    unet.train()

    for epoch in epochs:
        epoch_loss = 0
        for batch in tqdm(dataloader):
            optimizer.zero_grad()

            # En supposant que votre dataloader fournisse des images et des cibles (non utilisé ici)
            images, _ = batch
            images = images.to(unet.device)

            # Générer un bruit aléatoire
            noise = torch.randn(images.shape).to(unet.device)

            # Forward du modèle
            timesteps = torch.randint(0, noise_scheduler.config.num_train_timesteps, (images.shape[0],), device=unet.device).long()
            noisy_images = noise_scheduler.add_noise(images, noise, timesteps)
            predicted_noise = unet(noisy_images, timesteps).sample

            # Calcul de la perte (erreur quadratique moyenne entre le bruit réel et le bruit prédit)
            loss = torch.nn.functional.mse_loss(predicted_noise, noise)

            # Backward et optimisation
            loss.backward()
            optimizer.step()
            epoch_loss += loss.item()

        epoch_loss /= len(dataloader)
        losses[epoch] = epoch_loss
        print(f"Epoch {epoch + 1}/{num_epochs}, Loss: {epoch_loss}")
```

Cette fonction affiche la perte moyenne de chaque époque. Pour entraîner le modèle, nous faisons le forward du modèle sur des timeteps aléatoires avec des données provenant d'un dataloader. Ensuite nous calculons la perte sur le bruit prédit pour optimiser les paramètres de notre modèle. Dans le notebook, nous avons utilisé le dataloader de deeplearning.ai, donc rien de nouveau sous le soleil 😛. Comme vous pouvez le voir, la bibliothèque Diffusers n'est pas magique et nécessite que nous implémentions certaines choses nous-mêmes.

Pour sauvegarder notre modèle pré-entraîné, une seule ligne est nécessaire :

```python
model.save_pretrained(pre_trained_model_path)
```

## Faire des inférences avec le modèle entraîné

Maintenant que nous avons appris notre modèle, nous pouvons le charger et l'utiliser sur un pipeline de la bibliothèque diffusers.
Tout d'abord, chargeons les paramètres de notre modèle :

```python
model = UNet2DModel.from_pretrained(pre_trained_model)
```

C'est assez facile ! Maintenant, déclarons notre pipeline et utilisons cuda si disponible :

```python
from diffusers import DDPMPipeline

pipeline = DDPMPipeline(unet=model, scheduler=noise_scheduler)
pipeline.to("cuda" if torch.cuda.is_available() else "cpu")
```

Et c'est tout, maintenant générer de nouvelles images est aussi simple que :

```python
generated_image = pipeline(batch_size=16, num_inference_steps=500)
```

J'ai créé un outil simple pour montrer les images générées (vous pouvez jeter un oeil au dépôt GitHub pour plus d'informations).
Visualisons donc le résultat :

```python
fig = plot_generated_images(generated_image.images, 4, 4)
fig.show()
```

![images générées](/assets/images/diffusers/tutorial_1_unconditional.png)

Nous y voilà, nous avons généré nos premiers exemples en utilisant la librairie diffusers.
Dans un prochain article de blog, nous verrons comment générer des exemples à partir d'une vérité de terrain (au lieu de tutoriels n'utilisant que du texte et de suivre le cours deeplearning ai).

J'espère que cela aidera et/ou inspirera certains d'entre vous.

A bientôt, Vincent.
