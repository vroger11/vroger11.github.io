---
layout: post
comments: true
title:  "Mon passage à Manjaro Linux"
ref: switch-manjaro
date:   2022-09-03 20:00:00 +0200
categories: blogue
lang: fr
---


J'ai commencé mon aventure KDE avec Fedora (pendant 6 mois) où l'interface graphique était plus buguée que KDE Neon où je suis resté durant un an environ et j'ai fini dans Kubuntu LTS avec des applications flatpaks. Cela fait trois ans que j'utilise Kubuntu, bien que j'apprécie certains de ses aspects (LTS, établi sur les paquets Ubuntu, facile à configurer et avec de nombreuses ressources) mon expérience quotidienne était peu commode. Chaque fois que j'actualisais mes pilotes NVIDIA, je craignais d'avoir une interface graphique inutilisable (écran totalement noir avec seulement des terminaux tty disponibles) car même si tous les paquets nécessaires pour le pilote NVIDIA ne sont pas dans les dépôts, l'installation est faite par apt. Pire encore, ces fichiers manquants peuvent prendre une semaine pour arriver dans les dépôts. Moins casse-tête, mais également ennuyeux, snap est forcé via apt dans la distribution Ubuntu (comme `apt install firefox` ou `apt install chromium` résultent en une installation via snap). Enfin, avoir un environnement KDE dépassé de plusieurs versions n'est pas l'idéal si vous souhaitez bénéficier des dernières corrections de bogues de l'environnement (leur équipe fait de gros efforts pour corriger de nombreux bogues et améliorer l'expérience utilisateur, vous pouvez consulter le [blog de Nate](https://pointieststick.com/author/pointieststi,ck/) pour avoir un aperçu de leurs efforts).


# Mes besoins


Avant de vous dire pourquoi j'ai choisi Manjaro KDE, je vais vous exposer mes besoins :

- une distribution stable qui dispose des applications nécessaires à tous mes besoins (je peux utiliser des flatpaks
pour les applications manquantes), qui ne casse pas lorsque j'actualise mes pilotes et qui nécessite un minimum
de maintenance manuelle,  

- un moyen facile d'installer mon système,

- une grande communauté avec de la documentation et des wikis,

- Je ne veux pas réinstaller mon système tous les 6 mois (c'est pour cela que j'étais sur Kubuntu LTS),
    
- un environnement KDE à jour, mais pas forcément la dernière version, sans ruptures ni bugs.

Compte tenu de ces exigences, Manjaro et openSUSE semblent être d'excellentes solutions. J'ai déjà utilisé la documentation et les wikis d'Arch/Manjaro pour résoudre certains de mes problèmes avec Kubuntu (c'est une ironie car il n'y avait rien sur les forums d'Ubuntu/Kubuntu). De plus, en 2021, Manjaro semble être une solution plus populaire : https://distrowatch.com/index.php?dataspan=2021 (donc peut-être plus de personnes pour aider en cas de problèmes).

# Mon expérience


Comme aucune distribution n'est parfaite, il y a de bonnes choses à prendre et de mauvaises à surmonter. Tout d'abord le processus d'installation, ce n'est pas la meilleure interface utilisateur qui existe, mais je n'ai eu aucun problème ni aucune difficulté, tout était fluide. Ensuite vient le premier logiciel à installer, en combinant les paquets officiels avec flatpak et AUR, je n'ai pas installé un seul paquet manuellement. C'est une première pour moi, le gestionnaire de logiciels recherche les mises à jour de toutes mes applications (une chose très pratique).

Viennent ensuite les premiers soucis pour finaliser mon installation, que j'ai résolus avec quelques recherches sur les forums/wikis Arch et Manjaro (je les ai listés à la fin de ce post pour les curieux). Enfin, viennent les routines et quelques changements d'habitudes.

Dans l'ensemble, cela a été une excellente expérience et je n'ai pas regretté mon changement. J'ai même installé Manjaro sur tous mes ordinateurs. Ce n'est sûrement pas une distribution pour débutants (car vous devez faire peu de maintenance), mais une fois mis en place l'expérience utilisateur est excellente comparée à Kubuntu LTS (en termes de performance et de stabilité du système). Si vous ne vous souciez pas de l'environnement système, alors PopOS est une excellente alternative (adaptée aux débutants et aux experts). De plus, je n'ai pas à m'inquiéter des mises à jour majeures entre les LTS comme avec Kubuntu, car Manjaro est une rolling release (l'inconvénient est la maintenance pour certaines mises à jour). Enfin, je n'ai pas peur de mettre à jour mon système, ce qui était ma principale raison de quitter Kubuntu !

Dans les deux sous-sections restantes, j'expose un retour plus détaillé en listant tous les avantages et inconvénients que j'ai dans mon utilisation quotidienne. Ensuite, j'énumère les problèmes que j'ai rencontrés et comment je les ai résolus (suivi par les sources que j'ai utilisées pour résoudre ces problèmes).


## Utilisation quotidienne

### Les avantages

- Même interface que dans mon Kubuntu, mais avec un plasma plus récent (ce qui m'a permis de bénéficier de fonctionnalités plus récentes).

- Vous pouvez sélectionner votre noyau Linux et le changer facilement, avec un accès aux derniers noyaux (LTS et non LTS, qui peuvent contenir des pilotes pour les dernières pièces de matériel).
    
- Pas de version plasma de pointe à moins qu'elle ne soit pas stable (comparé à KDE Neon), c'est une victoire claire pour mes besoins.
    
- Mon système fonctionne de manière beaucoup plus fluide sur mon ordinateur portable par rapport à Kubuntu avec la même autonomie de batterie !

- Le gestionnaire de paquets a une interface similaire à `apt` : `pamac` (et non le gestionnaire `pacman`). Par conséquent, il m'aide beaucoup pour ma transition, de plus `pamac` suggère des paquets optionnels à installer (ce qui peut vous faire gagner du temps si vous manquez quelque chose).

- L'interpréteur de commandes par défaut de Manjaro, zsh, est pratique et utile pour moi (j'ai même basculé de bash vers lui).
    
- Un large panel de logiciels et des intégrations prêtes à l'emploi (comme l'intégration de languagetool + texstudio si vous installez les deux).

- Les mises à jour NVIDIA se sont bien passées (même après 10 mois d'utilisation).

### Inconvénients
 
- Les paquets AUR peuvent être cassés lors de certaines mises à jour. Cela arrive car ce n'est pas une source officielle de paquets supportés par l'équipe Manjaro (mais y avoir accès est très pratique). Comme j'utilise d'abord les paquets officiels, puis les applications flatpak et en dernier recours les paquets AUR, j'ai peu d'applications AUR, et ce n'est pas un problème pour moi. Néanmoins, les solutions pour résoudre ce problème peuvent être :
 
    - Retarder la mise à jour d'un paquet AUR pour conformer ses dépendances dans les dépôts de Manjaro (car elles arrivent plus tard dans Manjaro que dans Arch). Je n'ai pas eu besoin de faire cela dans mon utilisation.
        
    - Vous pouvez avoir besoin de lancer manuellement une reconstruction d'un paquet AUR (`pamac remove <paquet>` puis `pamac build <paquet>`) après une mise à jour des dépendances depuis les dépôts de Manjaro. Cela m'est arrivé une fois jusqu'à maintenant.
        
- Certaines mises à jour peuvent casser certains de vos fichiers de configuration (car le format peut changer avec les nouvelles versions majeures). Cela peut se produire entre chaque mise à jour de Kubuntu LTS, mais maintenant que je suis sur une distribution basée sur Arch, cela peut se produire à chaque mise à jour (pour chaque configuration spécifique que vous définissez sur votre système). En pratique, cela ne m'est arrivé qu'une fois à cause d'une mise à jour de gnome-keyring (dont j'espère pouvoir me débarrasser avec plasma 5.26, car kwallet implémentera les protocoles manquants gérés par gnome-keyring). Une bonne habitude pour résoudre ce genre de problèmes est de suivre le [flux officiel des versions de Manjaro (c'est un flux RSS)](https://forum.manjaro.org/c/announcements/stable-updates/12). Là, ils décrivent les mises à jour avec les problèmes possibles et la plupart du temps des solutions pour les résoudre. Comme chaque version est associée à un post sur le forum de Manjaro, vous pouvez demander de l'aide à la communauté (s'il n'y a pas encore de solution 😄).

- Vous ne pouvez pas utiliser discover pour installer et mettre à jour pour les nouvelles applications. J'étais habitué à cet outil, et cela s'intègre bien dans l'environnement KDE. Mais sur les systèmes basés sur Arch, son utilisation peut casser votre système. Heureusement, il existe un gestionnaire similaire sur Manjaro qui répond à tous mes besoins (recherche d'applications, de paquets et gestion des flatpaks). Sauf qu'il utilise une interface utilisateur gtk au lieu d'une interface qt, donc c'est un problème mineur.


# Problèmes rencontrés et solutions :

## Activer les périphériques Bluetooth sur l'écran de connexion

Cette fonctionnalité est très utile pour les utilisateurs de souris et/ou claviers Bluetooth (pour saisir les identifiants et sélectionner les utilisateurs). Je pense qu'elle devrait être activée par défaut, mais vous pouvez l'activer rapidement :

``bash
sudo sed -i.back 's/#AutoEnable=false/AutoEnable=true/g' /etc/bluetooth/main.conf
```

## Haut-parleur/écouteur Bluetooth grésillant avec une mauvaise qualité

Pour résoudre ce problème, désactivez d'abord toutes les options d'économie d'énergie sur les périphériques Bluetooth (uniquement pour les utilisateurs de [tlp](https://linrunner.de/tlp/index.html)). Pour faire cela facilement, vous pouvez installer `tlpui` :

``bash
pamac install tlpui
```

Ensuite, toujours dans tlpui -> section audio, désactivez l'option "sound power save controller". Cela devrait améliorer certains craquements, mais ce n'était pas suffisant de mon côté.
Vous pouvez aussi regarder dans USB_DENYLIST pour ajouter votre récepteur Bluetooth (même si vous utilisez un ordinateur portable ou que le Bluetooth est intégré à votre carte mère).

J'ai combiné cette solution avec le remplacement de pulseaudio par pipewire (qui fournit des connexions à faible latence, implémente plus de codecs Bluetooth et gère tous les appels pulseaudio car pipewire est compatible avec presque toutes les API pulseaudio). Plus d'informations sur pipewire [ici](https://pipewire.org/).

Pour le faire sous Manjaro, vous devez taper les commandes suivantes :

``bash
pamac remove pulseaudio pulseaudio-jack pulseaudio-lirc pulseaudio-rtp pulseaudio-zeroconf pulseaudio-bluetooth pulseaudio-alsa pulseaudio-ctl manjaro-pulse plasma-pa
pamac install manjaro-pipewire
pamac install plasma-pa
```

Au moment où j'écris ces lignes, nous devons d'abord supprimer `plasma-pa` pour pouvoir supprimer pulseaudio. Nous le réinstallons une fois que pipewire est installé pour pouvoir contrôler l'audio via l'interface plasma de KDE.

Puis redémarrez votre ordinateur et c'est fait. Toutes les applications utilisant précédemment pulseaudio devraient fonctionner avec une meilleure latence et en utilisant de meilleurs codecs 😄.


## Adresser les lags des périphériques Bluetooth tel que les lags de souris Bluetooth

Tout d'abord, si vous utilisez TLP, désactivez la fonction powersave pour votre périphérique à l'aide de tlpui (regardez la liste USB_DENYLIST pour ajouter votre récepteur Bluetooth). Ensuite, pour résoudre ce problème, nous allons définir un service qui réduit la latence Bluetooth au niveau du noyau Linux pour tous les périphériques. Enfin, nous aborderons les codecs Bluetooth pour les haut-parleurs et leurs interférences avec les autres périphériques Bluetooth.

### Définir un service pour forcer une faible latence
Pour ce faire, nous devons ajouter le script suivant dans `/etc/systemd/system/fix-mouse-lag.service` pour créer notre service :

``bash
[Unité]
Description=exécuter le script racine au démarrage/réveil pour corriger le décalage de la souris
Avant=bluetooth.service

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/bin/sleep 2
ExecStart=/usr/local/bin/fix-mouse-lag.sh

[Installer]
WantedBy=bluetooth.service
```

Ensuite, ajoutez le script suivant dans `/usr/local/bin/fix-mouse-lag.sh` qui sera exécuté par notre service à chaque démarrage/réveil de veille :

```bash
#!/bin/sh 

echo 0 > /sys/kernel/debug/bluetooth/hci0/conn_latency
echo 6 > /sys/kernel/debug/bluetooth/hci0/conn_min_intervalle
echo 7 > /sys/kernel/debug/bluetooth/hci0/conn_max_interval
```

Enfin, activons et démarrons notre service :

```bash
sudo chown root:root /etc/systemd/system/fix-mouse-lag.service
sudo chmod a+rx /usr/local/bin/fix-mouse-lag.sh
sudo systemctl enable fix-mouse-lag.service --now
```

### Sélection d'un codec Bluetooth pour mon casque audio

Je possède un Bose QC 35 II, et sur le papier il ne peut être utilisé qu'avec les codecs HSP/HFC, AAC et SBC. Le premier étant un codec de basse qualité qui permet d'utiliser le microphone du casque audio, et les derniers sont réservés à une sortie audio de haute qualité. Avec mes tests, j'ai découvert qu'il peut aussi gérer le codec SBC XQ (qui est un meilleur codec comparé à AAC et SBC, voir [ce lien](http://soundexpert.org/articles/-/blogs/audio-quality-of-sbc-xq-bluetooth-audio-codec) pour plus de détails).

J'ai testé ces trois codecs de haute qualité avec une Logitech MX Vertical et j'ai constaté que si les codecs SBC et SBC XQ ne perturbent pas la connexion de ma souris, le codec AAC, lui, le fait. Pipewire utilise le codec AAC par défaut, car Bose le recommande pour ce casque [^1]. Pour modifier ce comportement, nous devons éditer le fichier `/usr/share/pipewire/media-session.d/bluez-monitor.conf`.
J'y ai décommenté la ligne `bluez5.enable-sbc-xq = true` et spécifié les `bluez5.codecs` comme suit :

```bluez5.codecs = [ sbc_xq ldac aptx aptx_hd aptx_ll aptx_ll_duplex faststream faststream_duplex ]```

J'ai donc supprimé la capacité SBC et AAC de Pipewire pour être sûr qu'il n'utilisera pas ces codecs avec mes appareils (le premier n'est pas un problème, mais je préfère une qualité supérieure pour mon casque 😛).

[^1] : Notez que je ne parle que pour le cas pipewire. Je ne l'ai pas testé pour pulseaudio car j'ai des problèmes de crépitement avec ce dernier.

## Corriger le bug des thèmes ne fonctionnant pas avec les applications flatpak

La plupart des applications flatpak n'auront pas leur thème suivant celui du système sur Manjaro KDE. En particulier les applications gtk et electron. Si ce problème n'est pas traité en amont sur Arch, les développeurs de Manjaro ne le corrigeront pas (voir mes sources pour plus de détails). Pour résoudre ce problème, nous devons taper les commandes suivantes :

``bash
flatpak override --filesystem=xdg-config/gtk-3.0:ro
flatpak override --filesystem=xdg-config/gtk-4.0:ro
```

Il ajoute aux applications flatpak les permissions minimales dont elles ont besoin pour voir le thème utilisé dans l'environnement KDE. Ensuite, vous devez installer le thème flatpak GTK correspondant à celui que vous utilisez sous votre système. Par exemple, si vous utilisez le thème breeze-dark sous KDE, vous devez installer la version du thème flatpak pour les applications gtk comme ceci :

``bash
flatpak -y install org.gtk.Gtk3theme.Breeze-Dark
```

# Mes sources

- [ Utilisation de AUR sous Manjaro ](https://forum.manjaro.org/t/howto-use-aur/116934)
    
- [ Activer le Bluetooth au démarrage ](https://archived.forum.manjaro.org/t/enable-bluetooth-at-login-screen-system-boot/146842)
    
- [ Réduire les délais de saisie de la souris ](https://archived.forum.manjaro.org/t/bluetooth-mouse-lag/99386/43)
    
- [Résoudre le problème de thématisation de flatpak (par votre serviteur)](https://forum.manjaro.org/t/add-out-of-the-box-flatpak-gtk-application-themes-for-kde-plasma-users/117103)

