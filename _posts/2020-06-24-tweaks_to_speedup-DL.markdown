---
layout: post
comments: true
title:  "Tweaks on your machine to speedup Deep Learning Algorithms"
ref: tips_ml_performances
date:   2020-06-24 08:00:00 +0200
categories: blog dev
category: blog
lang: en
---

Learning a Deep Learning model can take a very long time to learn.
Indeed, it is possible to take a month to learn a state-of-the-art model (depending on your hardware available).
Lately, I searched how to improve speed time of some Deep Learning algorithms as even 1% improvement can lead a great deal.
In this post, I will share configurations I made on my systems.
Ubuntu-based systems (mostly derivatives).
These configurations save me time and here they are.

# Mount data disk with noatime option
This trick is the most important I have found.
By default, Linux file systems use the atime option.
It consists in writing the last access time in every read files.
This induces high IO disk usage when learning on large datasets with many files.
In my experiments, I gain roughly 5% of computing time on a training set of around 30000 audio files (with each audio being less than 10s).
More files will yield to improve this gain.
Remenber that having a High-performance data management like hdf5 is a good alternative to this solution (and may yield to greater performances).

In the next subsections, I will explain how to set the option noatime to a disk to disable the atime behavior.

## Identify the disk id
The id of a disk is called UUID.
The best way I know to identify a disk UUID is to type the following command:
```bash
sudo lsblk -fm
```

## Check if the disk is already mounted
Now we have identified the UUID, we need an unmounted disk.
To check this, just type the following command:
```bash
df -h
```

To unmount mounted disk, you have to type the following command:
```bash
umount <mounted point>
```

**Note:** If you want to apply the noatime on your system disk you still can modify your `/etc/fstab` file and it will be applied after a restart.

## Create mount point
Before mounting your data disk, you have to create the folder from which you will access to your data disk (or already use an existing one).
To do so, just type:

```bash
mkdir <your access destination>
```

## Create a fstab entry for the disk
Now we have the UUID of your data disk (which is unmounted) and a mount point, we will edit the `/etc/fstab` file.
Doing so, it will mount automatically your data disk after each restart.
To do so, add the following line in the `/etc/fstab` file:
```bash
UUID=<UUID-identified> <absolute_path_mount_point> ext4 errors=remount-ro,noatime  0 0
```

**Note:** the trick here is to use the noatime option.
Add it on system partitions (by modifying lines in the `/etc/fstab` file).

## Mount the disk
As we configured the `fstab` file, the system will automatically mount the disk (with the `noatime` option) after each reboot.
If you don't want to restart your machine, you can mount the partition for your data as follows:

```bash
sudo mount <absolute_path_mount_point>
```

# Have the latest driver
Another trick I use is to have an up-to-date graphic driver.
Hence, I can benefit from the last optimizations.
Since July 2019, Ubuntu offers the latest Nvidia drivers to LTS users.

To see available drivers, you need to type the following command:
```bash
ubuntu-drivers devices
```

Then you choose the last driver (let's say the 440) and use `apt` to install it:
```bash
sudo apt install nvidia-driver-440
```

# Use models with smaller precision

By default, Deep Learning libraries (like TensorFlow or PyTorch) use 64bits floats for each variable (hence weights of models).
To be able to have larger batch size (in case of large models) and be able to benefit from tensor cores (on last Nvidia GPUs), you can encode the weights into 32bits or 16bits.

**Note:** mixed precision (not explained here) is a better solution than fixing variable precision to 16bits.

## PyTorch

To use 32bits variables by default in PyTorch, you have to add the following line before creating your model:
```python
torch.set_default_tensor_type(torch.cuda.FloatTensor)
```

## TensorFlow

To use 32bits variables by default in TensorFlow/Keras, you have to add the following line before creating your model:
```python
tf.keras.backend.set_floatx("float32")
```

# Improve compilation time

To improve compilation time, you can use your RAM instead of your ROM (useful if you do not have an SSD).
To do so, you have to add the following line to your `/etc/fstab` file:

```bash
none    /tmp/    tmpfs    noatime,size=10%    0    0
```

**Note:** this limits the `/tmp` partition size (by 10% of the max RAM) and can block large usage of the `/tmp` cache (such as building singularity images).
Don't forget to change the cache to use for these cases (or increase the size of `/tmp` in the line above depending on your RAM available).

Hope it helps some of you.
If you have advices or more trick don't hesitate to share your knowledge 😄.

Cheers, Vincent
