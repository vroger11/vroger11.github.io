---
layout: post
comments: true
title:  "Formater une clé USB d'amorçage"
ref: format_bootable_key
date:   2020-01-31 12:30:00 +0200
categories: blogue
category: blogue
lang: fr
---

Cet article de blogue est surtout un rappel pour moi, car je lutte à chaque fois pour arriver à cette solution :stuck_out_tongue_winking_eye:.
Après avoir créé une clé d'amorçage avec ma distribution préférée (KDE Neon, si vous n'aviez pas deviné), je reçois un message d'erreur lorsque je veux la formater en utilisant une interface graphique (testé avec gnome-disk, kde partition manager et gparted).

Voici les étapes que je dois faire pour être capable de modifier la clé USB avec une interface graphique de formatage.

# Identifier le nom de clé USB dans votre système

D'abord, tapez la commande suivante:

```bash
lsblk
```

Cela vous donne une sortie comme suit:
```bash
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    1  28,7G  0 disk
└─sda1        8:1    1  28,6G  0 part /media/vincent/Kde Neon
nvme0n1     259:0    0 126,9G  0 disk
├─nvme0n1p1 259:1    0   512M  0 part /boot/efi
└─nvme0n1p2 259:2    0 126,4G  0 part /
```

Ici, ma clé d'amorçage a le nom sda.

## Remplir la clé de zéros

Soyez vigilants dans cette partie, vous pouvez endommager votre système d'exploitation en cas d'erreurs!
Maintenant, il vous faut taper les commandes suivantes et remplacer `/dev/sda` par `/dev/<le nom de votre clé>`.

```bash
sudo dd if=/dev/zero of=/dev/sda bs=4k conv=fsync && alert
```

Et c'est fini.
Maintenant, vous pouvez utiliser votre interface favorite pour formater votre clé usb.
La commande `&& alert` n'est pas nécessaire.
Si vous voulez comprendre son intérêt, je vous suggère de regarder mon [article de blog sur la configuration bash](/blogue/dev/2019/09/23/multiplexeur-de-terminaux.html).

J’espère que cela aidera certains d’entre vous, cela m'aide certainement :smile:.

À la revoyure, Vincent.
