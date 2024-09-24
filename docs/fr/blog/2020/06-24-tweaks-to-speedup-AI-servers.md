---
comments: true
title:  "Astuces pour accélérer le Deep Learning sur les serveurs"
date:   2020-06-24
tags:
    - Développement
hide:
    - navigation
excerpt: Cela va de la façon dont monter le disque de données, passant par la mise à jour de pilotes, puis changer la précision du modèle, pour terminer par les temps de compilation à l'aide d'astuces.
---

Lancer l'apprentissage d'un Réseau de neurones profonds peut prendre énormément de temps.
En effet, il n'est pas impossible de nécessiter un mois pour apprendre un modèle de l'état de l'art (tout dépend du matériel à votre disposition).
Dernièrement, j'ai cherché à accélérer les temps d'apprentissage de modèles basé sur des réseaux profonds vu que même 1% d'amélioration est toujours à prendre.
Dans cet article de blogue, je vais partager les configurations que j'effectue sur toutes les machines mises à ma disposition (basées sur Ubuntu).
Ces configurations me permettent de gagner du temps et les voici.

## Monter le disque de données avec l'option noatime

Cette astuce est la plus importante que j'ai trouvée.
Par défaut, le système de fichier Linux utilise l'option atime.
Cette option consiste à écrire le dernier temps d'accès dans chaque fichier lu.
Cela induit un haut usage d'entrées/sorties disque lorsque l'on apprend un modèle sur de larges bases de données avec une multitude de fichiers.
Lors de mes expériences, je gagne environ 5% de temps de calcul sur un ensemble d'entrainement d'environ 30000 fichiers audios (durant moins de 10s chacun).
Plus de fichiers résulteraient à de meilleurs gains.
Gardez à l'esprit qu'utiliser un manager de données de haute performance (tel que hdf5) est une bonne alternative à cette solution (et peut potentiellement obtenir de meilleures performances).

Dans les prochaines sous-sections, je vais expliquer comment mettre l'option noatime sur un disque pour désactiver le comportement atime.

### Déterminer l'identifiant d'un disque

L'identifiant d'un disque se nomme UUID.
Identifier l'UUID d'un disque peut se faire facilement en utilisant la commande suivante:

```bash
sudo lsblk -fm
```

### Vérifier si le disque est déjà monté

Maintenant que nous avons l'UUID de notre disque, nous devons nous assurer qu'il ne soit pas déjà monté.
Pour cela, tapez la commande suivante:

```bash
df -h
```

Si le disque est déjà monté, vous devez le démonter:

```bash
umount <point d accès>
```

!!!info "Note"
    Si vous voulez appliquer l'option noatime sur le disque de votre système d'exploitation, vous pouvez toujours modifier le fichier `/etc/fstab` et cela sera appliqué au prochain redémarrage de votre ordinateur.

### Créer un point d'accès

Avant de monter votre disque, vous devez créer dossier servant de point d'accès (ou utiliser un dossier déjà existant).
Pour créer un dossier, tapez:

```bash
mkdir <votre point d accès>
```

### Créer une entrée fstab pour votre disque

Maintenant que nous avons l'UUID de votre disque de données (qui est non monté) et un point d'accès, nous allons éditer le fichier `/etc/fstab`.
En faisant comme ceci, votre disque de données se montera automatiquement lors des prochains redémarrages.
Pour ce faire, ajoutez la ligne suivante dans le fichier `/etc/fstab`:

```bash
UUID=<UUID-identifié> <chemin absolu du point d accès> ext4 errors=remount-ro,noatime  0 0
```

!!!info "Notes"
    L'astuce ici est d'ajouter l'option noatime.
    Elle peut être ajoutée sur les partitions système (en modifiant les lignes du fichier `/etc/fstab`).

### Monter le disque de données

Nous venons de configurer le fichier `fstab`, ce qui montera votre disque au prochain redémarrage.
Si vous ne voulez pas redémarrer votre machine, vous pouvez monter votre disque de données grâce à la commande suivante:

```bash
sudo mount <chemin absolu du point d accès>
```

## Avoir les derniers pilotes

Une autre astuce que j'utilise est de toujours avoir les pilotes de la carte graphique à jour.
Ainsi, je peux bénéficier des dernières optimisations.
Depuis juillet 2019, Ubuntu offre l'accès aux derniers pilotes Nvidia pour leurs utilisateurs de version LTS (support à long terme).

Pour lister les pilotes disponibles, vous devez taper la commande suivante:

```bash
ubuntu-drivers devices
```

Ensuite, vous choisissez le dernier driver (disons le 560) et utilisez la commande `apt` comme suivant:

```bash
sudo apt install nvidia-driver-560
```

## Suivre les recommendations de votre librairie de deep learning

Il existe de nombreuses techniques pour optimiser le temps d'entraînement et d'inférence de vos modèles. Bien qu'il existe des techniques spécifiques à chaque librairie, beaucoup sont communes à PyTorch et TensorFlow.

Parmi les plus utiles dans ces deux frameworks, on peut citer l'utilisation de la précision mixte et la compilation du réseau pour améliorer considérablement la vitesse d'apprentissage de vos modèles. La précision mixte combine des calculs en précision simple (32 bits) et en demi-précision (16 bits), ce qui accélère l'entraînement tout en réduisant la consommation de mémoire, sans compromettre la précision des résultats. La compilation, quant à elle, optimise le calcul des graphes pour les rendre plus efficaces. Cela permet une exécution plus rapide sur les CPU et GPU, en minimisant les calculs redondants et en maximisant le parallélisme.

Pour aller plus loin, vous pouvez explorer la quantization (réduction du nombre de bits pour encoder les paramètres) et le prunage de modèles (suppression de paramètres tout en conservant la performance).

Retrouvez plus de détails sur la mise en œuvre de ces techniques [ici pour TensorFlow](https://www.tensorflow.org/model_optimization) et [ici pour PyTorch](https://pytorch.org/tutorials/recipes/recipes/tuning_guide.html).

## Améliorer les temps de compilation

Pour améliorer les temps de compilation, vous pouvez utiliser la RAM au lieu d'un disque physique (encore plus utile si vous n'avez pas de SSD).
Pour ce faire, vous devez ajouter la ligne suivante dans votre fichier `/etc/fstab`:

```bash
none    /tmp/    tmpfs    noatime,size=10%    0    0
```

!!!info "Notes"
    Ceci limite la taille maximale de `/tmp` (de 10% de la quantité maximale de RAM) et peut bloquer les usages nécessitant un large cache (comme construire des images singularity).
    N'oubliez pas de changer le cache utilisé pour ces cas (ou augmentez la taille de `/tmp` dans la ligne au-dessus et en fonction de votre quantité de RAM disponible).

J’espère que cela aidera certains d’entre vous.
Si vous avez des conseils ou d'autres astuces, n'hésitez pas à partager votre savoir 😄.

À bientôt, Vincent.
