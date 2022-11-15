---
layout: post
comments: true
title:  "Automatisation en utilisant des clés SSH"
ref: ssh_keys
date:   2020-08-02 08:00:00 +0200
categories: blogue automatisation
category: blogue
lang: fr
---

J'ai commencé mon blogue avec un article de blogue sur comment sauver les identifiants git pour les sites internet ne supportant pas les clés SSH (c'est par [ici](/blogue/dev/2019/09/09/sauver-les-identifiants-git.html) pour ceux qui sont intéressés).
À ce moment, j'étais dans l'impossibilité d'utiliser les clés SSH sur les serveurs d'Overleaf (et c'est toujours le cas au moment où j'écris).
Cela m'a aidé pour ce cas particulier.
Néanmoins, cette astuce fonctionne uniquement pour les serveurs git.
De plus, ce n'est pas le meilleur moyen pour automatiser ses connexions (au moins de mon point de vue).
Aujourd'hui, nous allons voir comment utiliser les clés SSH pour automatiser plusieurs étapes d'identifications.

Dans cet article de blogue, nous allons voir deux cas d'utilisation:
* Identification automatique à des serveurs utilisant le protocole SSH.
* Identification automatique lors de commandes push/push sur des serveurs git (comme GitHub ou GitLab).

Les clés SSH sont composées d'une clé publique pour encoder les messages (destiné aux serveurs) et une clé privée afin de lire ces messages (destiné pour le client du serveur).
Si vous voulez plus d'information sur ce protocole, allez voir [ici](https://delicious-insights.com/fr/articles/comprendre-et-maitriser-les-cles-ssh/).

La distribution utilisée (et testée) pour ce tutoriel est Kubuntu 20.04 LTS (ma nouvelle distribution, mais c'est pour un futur article de blogue).

# Préparer le côté client

Dans cette section, nous allons préparer le client qu'il puisse s'identifier automatiquement sur les serveurs.
D'abord, créons une paire de clés publique et privée.
Enfin, nous démarrerons l'agent de clés SSH pour le configurer avec la clé privée.

## Vérifiez la présence d'une paire de clés SSH

Pour cela il faut regarder dans le dossier `~/.ssh/` pour voir si vous avez déjà une clé publique SSH.
Par défaut, le nom de fichier d'une clé publique est `id_rsa.pub`.
Vous pouvez utiliser une paire différente de clés par serveur si vous voulez.
Je préfère garder une paire de clés par machine (et en changer régulièrement).

## Générer une paire de clés SSH

Pour générer une paire de clés liées à une adresse email (pour mieux identifier l'utilisateur connecté) vous devez écrire la ligne suivante:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Ensuite, vous devez suivre les instructions.
Vous pouvez laisser le chemin des clés par défaut si vous n'avez pas déjà une paire de clés.
Vous allez également définir un mot de passe pour déverrouiller votre clé privée, soyez certain de vous en rappeler.
**Faites également attention de ne pas révéler votre clé privée (le fichier `id_rsa`) en toutes circonstances.**

## Démarrer l'agent de clé SSH et ajouter votre clé privée

Pour laisser votre système se souvenir de votre clé privée durant votre session, vous pouvez utiliser l'agent SSH comme suit:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Maintenant, nous avons une paire de clés privée et publique ainsi que l'agent de clé SSH de configurés.
Nous sommes prêts pour automatiser les identifications SSH vers des serveurs SSH et/ou serveur git.

# Automatisez votre connexion sur les serveurs en utilisant SSH

Pour automatiser l'étape de saisie de votre mot de passe, vous pouvez ajouter votre clé publique dans la liste des clés autorisées de vos serveurs.
Avant ça, assurez-vous que votre espace utilisateur distant contienne le dossier `~/ssh`:
```bash
ssh <identifiant>@<adresse_serveur> mkdir -p .ssh
```

Ensuite, vous devez ajouter votre clé publique dans le fichier des clés autorisées de vos serveurs:
```bash
cat ~/.ssh/id_rsa.pub | ssh <identifiant>@<adresse_serveur> 'cat >> .ssh/authorized_keys'
```

Maintenant, votre serveur est configuré avec votre paire de clés SSH.
Vous pouvez répéter ces deux étapes pour chaque serveur dont vous avez accès.

# Automatiser vos authentifications sur les serveurs Git

Pour automatiser vos authentifications (écrire votre identifiant et votre mot de passe) après l'utilisation d'une commande `push`/`pull`, vous devez ajouter votre clé publique dans le serveur git (en utilisant l'interface web du serveur git).
Pour copier votre clé publique sur un site internet (tel que GitHub ou GitLab) vous pouvez ajouter votre clé dans le presse-papier (pour utiliser Ctrl+V à l'intérieur de votre navigateur internet).

À cette fin, vous devez installer `xclip`:
```bash
sudo apt install xclip
```

ou si vous êtes sur manjaro:
```bash
sudo pamac install xclip
```

Ensuite, c'est aussi simple que suit:
```bash
xclip -sel clip < ~/.ssh/id_rsa.pub
```

Après, suivez les étapes de ce [lien](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) (en anglais, mais très bien illustré) pour GitHub et de cet autre [lien](https://docs.gitlab.com/ee/ssh/#add-an-ssh-key-to-your-gitlab-account) (en anglais, mais la documentation officielle n'est pas traduite en français) pour GitLab.

Maintenant, votre configuration est prête pour votre serveur Git.
La prochaine sous-section expliquera comment tester cette configuration sans avoir à modifier l'un de vos répertoires.

## Tester votre configuration

Pour tester votre nouvelle configuration sur vos serveurs git, vous pouvez utiliser le protocole SSH.
Cela vous évitera de modifier un de vos répertoires distants.
Les prochaines sous-sections vous montrent comment faire avec les serveurs les plus populaires.

### GitHub

```bash
ssh -T git@github.com
```
### GitLab

```bash
ssh -T git@gitlab.com
```

# Garder les identités de l'agent SSH sur Kubuntu 22.04 après un redémarrage

Avec les précédentes instructions, vous devez (du moins dans Kubuntu) reconfigurer votre agent SSH après chaque redémarrage ou déconnexion.
Dans cette sous-section, nous allons utiliser kwallet pour outrepasser cette limitation.

D'abord, nous devons installer le paquet `ssh-askpass`:
```bash
sudo apt install ssh-askpass
```

Ensuite, nous devons créer un script qui va automatiquement déverrouiller votre clé privée lorsque vous vous connectez.
Ceci est fait par les lignes suivantes:
```bash
mkdir -p ~/.config/autostart-scripts
echo '#!/bin/sh' > ~/.config/autostart-scripts/ssh-add.sh
echo 'export SSH_ASKPASS=/usr/bin/ksshaskpass' >> ~/.config/autostart-scripts/ssh-add.sh
echo 'ssh-add < /dev/null' >> ~/.config/autostart-scripts/ssh-add.sh
chmod +x ~/.config/autostart-scripts/ssh-add.sh
```

L'étape suivante consiste à taper la commande suivante et de **cocher la case pour se souvenir du mot de passe**:
```bash
~/.config/autostart-scripts/ssh-add.sh
```
Cela va permettre à kwallet (portefeuille de clés de KDE) de retenir le mot de passe pour votre clé privée et la débloquer après chaque connexion.

# Garder les identités de l'agent SSH sur Manjaro après un redémarrage

Après mon passage sur Manjaro linux ([voir mon post sur ma migration](/blogue/2022/09/03/mon-passage-a-manjaro.html)), je me suis aperçu que la méthode pour Kubuntu ne fonctionnait pas (et risque de ne pas fonctionner sur les prochaines LTS). Voici ma solution, qui est une combinaison de deux approches que vous pouvez retrouver dans mes sources.

Tout d'abord, assurez-vous d'avoir le nécessaire :

```bash
sudo pamac install kwallet ksshaskpass kwalletmanager
```

Ensuite, configurons notre système et zsh pour utiliser les voix de communications adaptées à l'agent ssh:
```bash
sudo echo "#!/bin/sh" > /etc/profile.d/ssh-askpass.sh
sudo echo "export SSH_ASKPASS=/usr/bin/ksshaskpass" >> /etc/profile.d/ssh-askpass.sh
echo "export SSH_AUTH_SOCK="$XDG_RUNTIME_DIR"/ssh-agent.socket" >> ~/.zsherc
```

Ensuite, nous créons le répertoire utilisateur pour systemd:

```bash
mkdir -p ~/.config/systemd/user
```

Ensuite, créez le fichier `~/.config/systemd/user/ssh-agent.service` et remplissez-le avec le contenu de l'encadré suivant :

```bash
[Unit]
Description=SSH agent (ssh-agent)

[Service]
Type=simple
Environment=SSH_AUTH_SOCK=%t/ssh-agent.socket
Environment=DISPLAY=:0
Environment=KEY_FILE=/home/%u/.ssh/id_rsa
ExecStart=ssh-agent -D -a $SSH_AUTH_SOCK
ExecStartPost=/bin/sleep 3
ExecStartPost=/usr/bin/ssh-add $KEY_FILE
ExecStop=kill -15 $MAINPID

[Install]
WantedBy=default.target
```

Ce service lance l'agent ssh à chaque authentification et ajoute la clé privée que vous avez créée en début de cet article.
Nous allons maintenant l'activer et le lancer :

```bash
systemctl --user daemon-reload
systemctl --user enable ssh-agent.service
systemctl --user start ssh-agent.service
```

Voilà, on est bon, en espérant que cela vous a été utile 😉.


# Sources et inspirations

* [Instructions officielles de GitHub pour les clés SSH](https://help.github.com/en/github/authenticating-to-github)
* [Instructions officielles de GitLab pour les clés SSH](https://docs.gitlab.com/ee/ssh/)
* [Configurer l'agent de clé SSH d'Ubuntu](http://www.linuxproblem.org/art_9.html)
* [Kubuntu et l'agent de clé SSH](https://wiki.csnu.org/index.php/Kubuntu_/_KDE_:_login_ssh_automatique_par_cl%C3%A9)
* [Manjaro et l'agent de clé SSH (1/2)](https://forum.manjaro.org/t/configuring-ssh-agent-to-autostart-and-automatically-add-ssh-keys-to-it/99715)
* [Manjaro et l'agent de clé SSH (2/2)](https://forum.manjaro.org/t/howto-use-kwallet-as-a-login-keychain-for-storing-ssh-key-passphrases-on-kde/7088)

J’espère que cela aidera certains d’entre vous.

À la revoyure, Vincent.
