---
comments: true
title:  "Tweaks to speedup Deep Learning on servers"
date:   2020-06-24
tags:
    - Development
hide:
    - navigation
excerpt: This ranges from how to mount the data disk, to updating drivers, changing the accuracy of the model and finish to boost compile times using tweaks.
---

Learning a Deep Learning model can take a very long time to learn.
Indeed, it is possible to take a month to learn a state-of-the-art model (depending on your hardware available).
Lately, I searched how to improve speed time of some Deep Learning algorithms as even 1% improvement can lead a great deal.
In this post, I will share configurations I made on my systems.
Ubuntu-based systems (mostly derivatives).
These configurations save me time and here they are.

## Mount data disk with noatime option

This trick is the most important I have found.
By default, Linux file systems use the atime option.
It consists in writing the last access time in every read files.
This induces high IO disk usage when learning on large datasets with many files.
In my experiments, I gain roughly 5% of computing time on a training set of around 30000 audio files (with each audio being less than 10s).
More files will yield to improve this gain.
Remenber that having a High-performance data management like hdf5 is a good alternative to this solution (and may yield to greater performances).

In the next subsections, I will explain how to set the option noatime to a disk to disable the atime behavior.

### Identify the disk id

The id of a disk is called UUID.
The best way I know to identify a disk UUID is to type the following command:

```bash
sudo lsblk -fm
```

### Check if the disk is already mounted

Now we have identified the UUID, we need an unmounted disk.
To check this, just type the following command:

```bash
df -h
```

To unmount mounted disk, you have to type the following command:

```bash
umount <mounted point>
```

!!!info "Note"
    If you want to apply the noatime on your system disk you still can modify your `/etc/fstab` file and it will be applied after a restart.

### Create mount point

Before mounting your data disk, you have to create the folder from which you will access to your data disk (or already use an existing one).
To do so, just type:

```bash
mkdir <your access destination>
```

### Create a fstab entry for the disk

Now we have the UUID of your data disk (which is unmounted) and a mount point, we will edit the `/etc/fstab` file.
Doing so, it will mount automatically your data disk after each restart.
To do so, add the following line in the `/etc/fstab` file:

```bash
UUID=<UUID-identified> <absolute_path_mount_point> ext4 errors=remount-ro,noatime  0 0
```

!!!info "Note"
    The trick here is to use the noatime option.
    Add it on system partitions (by modifying lines in the `/etc/fstab` file).

### Mount the disk

As we configured the `fstab` file, the system will automatically mount the disk (with the `noatime` option) after each reboot.
If you don't want to restart your machine, you can mount the partition for your data as follows:

```bash
sudo mount <absolute_path_mount_point>
```

## Have the latest driver

Another trick I use is to have an up-to-date graphic driver.
Hence, I can benefit from the last optimizations.
Since July 2019, Ubuntu offers the latest Nvidia drivers to LTS users.

To see available drivers, you need to type the following command:

```bash
ubuntu-drivers devices
```

Then you choose the last driver (let's say the 560) and use `apt` to install it:

```bash
sudo apt install nvidia-driver-560
```

## Follow the recommendations of your Deep Learning library

There are numerous techniques to optimize the training and inference time of your models. While some techniques are specific to each library, many are common to both PyTorch and TensorFlow.

Among the most useful in these two frameworks are the use of mixed precision and model compilation to significantly improve training speed. Mixed precision combines single precision (32-bit) and half precision (16-bit) computations, which speeds up training while reducing memory usage, without sacrificing result accuracy. Compilation, on the other hand, optimizes graph computation to make it more efficient. This allows for faster execution on CPUs and GPUs, by minimizing redundant calculations and maximizing parallelism.

To go further, you can explore quantization (using fewer bits to encode parameters) and model pruning (removing parameters while maintaining performance).

You can find more details on implementing these techniques [here for TensorFlow](https://www.tensorflow.org/model_optimization) and [here for PyTorch](https://pytorch.org/tutorials/recipes/recipes/tuning_guide.html).

## Improve compilation time

To improve compilation time, you can use your RAM instead of your ROM (useful if you do not have an SSD).
To do so, you have to add the following line to your `/etc/fstab` file:

```bash
none    /tmp/    tmpfs    noatime,size=10%    0    0
```

!!!info "Note"
    This limits the `/tmp` partition size (by 10% of the max RAM) and can block large usage of the `/tmp` cache (such as building singularity images).
    Don't forget to change the cache to use for these cases (or increase the size of `/tmp` in the line above depending on your RAM available).

Hope it helps some of you.
If you have advices or more trick don't hesitate to share your knowledge 😄.

Cheers, Vincent
