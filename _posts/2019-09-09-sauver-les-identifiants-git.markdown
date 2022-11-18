---
layout: post
comments: true
title:  "Sauver les identifiants git"
ref: git_credentials
date:   2019-09-09 19:30:00 +0200
categories: blogue automatisation
category: blogue
lang: fr
---

# Qu'est-ce que les identifiants git?

Les identifiants git sont le nom d'utilisateur et le mot de passe associé que vous utilisez pour vous connecter sur un serveur git.
Les sauvegarder vous permet d'éviter de les taper lorsqu'ils sont requis (principalement lors d'un push ou d'un pull).

# Pourquoi sauver ces identifiants?
L'utilisation de clés SSH ou même GPG est disponible sur la plupart des serveurs git (tel GitLab ou même sur GitHub).
Cela permet également de se connecter sans taper son nom d'utilisateur et son mot de passe.

Certains sites internet ne supportent pas cette fonctionnalité (les serveurs git d'overleaf par exemple).
Aussi, si vous êtes comme moi, vous utilisez une multitude de serveurs git (et configurer chaque clé SSH devient lourd).

# Sauver ses identifiants

Pour sauver ses identifiants pour chaque dépôt auquel vous vous connectez, il suffit de taper juste avant:

```bash
git config credential.helper store
```

Et c'est tout.
Vous allez remplir seulement une fois vos identifiants pour chaque dépôt.
Les prochaines fois, ils seront remplis automatiquement.

# Le cache d'expiration

Pour des raisons de sécurité (surtout si vous n'êtes pas sur votre ordinateur personnel), il est préférable de sauvegarder vos identifiants pour un temps limité.
Disons qu'un délai de 4 heures vous semble approprié.
Il vous suffit donc taper cette commande:

```bash
git config --global credential.helper 'cache --timeout 14400'
```

14400 est le temps en secondes (nos 4 heures)

J'espère que cela aidera certains d'entre vous.

À la revoyure, Vincent.
