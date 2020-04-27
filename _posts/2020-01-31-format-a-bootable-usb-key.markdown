---
layout: post
comments: true
title:  "Format a bootable USB key"
ref: format_bootable_key
date:   2020-01-31 12:30:00 +0200
categories: tips
lang: en
---

This post is mostly a remainder for me as it struggles me every time :stuck_out_tongue_winking_eye:.
After making a USB key bootable with any distribution, I get an error message when I want to format it with a graphical interface (tested with gnome-disk, kde partition manager and gparted).

This is the steps I have to do in order to be able to modify this USB key using one of the graphical interfaces I listed.

# Identify the USB key name on your system

First type the following command:

```bash 
lsblk
```

It gives you an output like this:
```bash
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    1  28,7G  0 disk 
└─sda1        8:1    1  28,6G  0 part /media/vincent/Kde Neon
nvme0n1     259:0    0 126,9G  0 disk 
├─nvme0n1p1 259:1    0   512M  0 part /boot/efi
└─nvme0n1p2 259:2    0 126,4G  0 part /
```

Here, my bootable key has the name sda.

## Fill the USB key with zeros

Be careful on this part, it can harm your system if you do it wrong!
Now you have to type the following command and replace `/dev/sda` by `/dev/<your usb name>`.

```bash
sudo dd if=/dev/zero of=/dev/sda bs=4k conv=fsync && alert
```

And it is done. Now you can use your favorite graphical interface to format your key.
The `&& alert` command is not necessary. If you want to understand what it is, I suggest you to look at [my post about my bash configuration](http://website.vincent-roger.fr/tips/dev/2020/01/29/improve-your-bash-navigation.html).

Hope it helps some of you, it surely helps me :smile:.

Cheers, Vincent.
