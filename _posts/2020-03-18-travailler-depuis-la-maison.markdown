---
layout: post
comments: true
title:  "Configurations pour travailler depuis la maison"
ref: remote_work
date:   2020-03-18 20:00:00 +0200
categories: astuces
lang: fr
---

Cet article de blogue présente toutes les configurations que j'ai effectuées pour travailler depuis chez moi.
C'est particulièrement utile lorsque vous êtes en quarantaine (ou allez être, car la progression du COVID-19 est alarmante).
Je vais décrire toutes les étapes pour les utilisateurs Ubuntu avec un environnement KDE, les manipulations pour les utilisateurs de GNOME étant similaire je n'irai pas en détail pour ce cas.

# Connecter votre ordinateur personnel au VPN de votre laboratoire/compagnie en utilisant OpenVPN

C'est une première étape requise si vous voulez avoir accès à l'Intranet de votre compagnie/laboratoire.
Mon laboratoire supporte OpenVPN, je vais décrire les étapes requises pour utiliser cet outil.

En premier, il faut installer OpenVPN:

```bash
sudo apt install openvpn
```

Ensuite, installez le connecteur de votre environnement de travail.

```bash
# pour les utilisateurs de KDE
sudo apt install network-manager-openvpn
# pour les utilisateurs de GNOME
sudo apt install network-manager-openvpn-gnome
```

Ensuite, vous pouvez utiliser l'interface graphique de votre environnement de travail pour vous connecter au VPN désiré.
Pour les utilisateurs de KDE, aller dans le widget de réseaux:

![widget réseaux](/assets/images/work_at_home/Network_widget.png)

Ensuite, cliquez sur l'icône "Configure network connections..."  (entouré de bleue sur l'image précédente).
Cela ouvre la fenêtre de configuration suivante:

![Menu réseaux](/assets/images/work_at_home/Network_menu.png)

Pour ajouter une connexion VPN, cliquez sur l'icône "Add new connection".
Cela vous ouvrira le menu suivant:

![Options VPN](/assets/images/work_at_home/VPN_options.png)

Ici, cliquez sur OpenVPN, puis cliquez sur "Create" et suivez les instructions de votre compagnie/laboratoire.
Si votre laboratoire/compagnie fournit un fichier OpenVPN, c'est encore plus simple.
Il vous faut cliquer sur "Import VPN connection..." et sélectionnez le fichier qui vous est fourni.

# Se connecter à un serveur distant

J'utilise une connexion ssh pour accéder aux machines de mon travail depuis chez moi.
Pour rappel, on se connecte comme suit à un serveur distant via ssh:

```bash
ssh <login>@<addresss_du_serveur> -p <numéro_de_port>
```

Si vous voulez créer un serveur ssh ou avoir plus de détails, je vous recommande de regarder la [documentation officielle d'Ubuntu](https://help.ubuntu.com/lts/serverguide/openssh-server.html).

Après m'être connecté, je commence toujours (ou attache) une session tmux.
Si vous ne connaissez pas ce qu'est tmux, allez regarder mon article de blog [ici](/astuces/dev/2019/09/23/multiplexeur-de-terminaux.html).
Pour automatiser ce comportement, j'ajoute le code suivant dans mon fichier `~/.bashrc` côté serveur:
```bash
# Ouvre tmux automatiquement lorsque la machine est accédé par ssh et sans serveur X
if [ ! -z "$SSH_CLIENT" ] && [ -z "$DESKTOP_SESSION" -a -z "$TMUX" ] ; then
    tmux attach -t "ssh" 2> /dev/null || tmux new -s "ssh"
fi
```

Maintenant, à chaque connexion via ssh, je me trouve dans une session tmux nommée `ssh`.

# Travailler avec des Notebooks de Jupyter Lab

Si vous utilisez Jupyter Lab, vous voulez certainement utiliser votre navigateur local pour accéder à vos notebooks.
Il y a une multitude de solutions disponible sur internet.
Ici je vous donne ma solution:
1. Créer un mot de passe pour les notebooks de jupyter (côté serveur):
    ```bash
    jupyter notebook password
    ```
2. Lancer jupyter lab en utilisant un port définit (ici 8887):
    ```bash
    jupyter lab --port=8887 --no-browser
    ```
3. Sur votre terminal local, il faut transmettre le port serveur 8887 vers un port local de votre machine (disons le port 8888).
    Pour cela j'utilise les tunnels ssh:
    ```bash
    ssh -N -f -L 8888:localhost:8887 <login>@<adresse_serveur>
    ```
4. Utiliser votre navigateur local et pour aller sur [localhost:8888/](localhost:8888/).
    Ensuite, entrez votre mot de passe créé en étape 1.

5. Profitez

Lorsque vous avez fini, vous voulez certainement arrêter le tunnel ssh sur votre machine locale.
Pour faire cela, il faut identifier le PID de la commande utilisée en étape 3 en utilisant la commande:
```bash
ps -ax | grep "ssh -N -f -L"
```
Maintenant que vous avez identifié le PID, il vous faut utiliser la commande `kill` pour arrêter le tunnel:
```bash
kill -9 <PID_identifié>
```

J’espère que cela aidera certains d’entre vous.

À la revoyure, Vincent.
