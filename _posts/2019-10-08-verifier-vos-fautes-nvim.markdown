---
layout: post
comments: true
title:  "Vérifier vos fautes dans Neovim"
ref: check_writing
date:   2019-10-08 07:15:00 +0200
categories: astuces
lang: fr
---

Quand j'écris du LaTeX, du Markdown ou du Python j'utilise Neovim.
Neovim est un logiciel ayant refactoré le code de Vim pour être plus accessible et plus extensible (tout en restant compatible avec la plupart des plugins de Vim).
Basiquement, Neovim peut être vu comme étant un Vim amélioré, allez sur [la page officielle du projet](https://neovim.io/charter/) pour plus de détails.
Après avoir écrit votre texte ou documentation, les correcteurs automatiques sont utiles pour vous faire gagner du temps de relecture.
Dans ce post je vous partage ma configuration Neovim pour faire cette relecture automatique.

# Correction d'orthographe

Pour avoir une vérification orthographique dans neovim, vous avez à ajouter dans le fichier `~/.config/nvim/init.vim` les lignes suivantes:

```
" spell languages
set spelllang=fr
nnoremap <silent> <C-s> :set spell!<cr>
inoremap <silent> <C-s> <C-O>:set spell!<cr>
```

## Utilisation

Une fois dans Neovim, appuyer sur Ctrl+S pour mettre en évidence les mots mal orthographiés.
Il vous suffit de rappuyer sur Ctrl+S pour désactiver cette fonctionnalité.
Pour avoir accès à une suggestion de mots, il vous faut appuyer Ctrl+X puis S lorsque vous êtes en mode insertion.
En mode normal, taper `z=` lorsque le curseur est sur un mot surligné pour afficher la suggestion de mots

Remarque: Je sais que ma configuration inhibe le signal utilisateur Ctrl+S et c'est justement mon objectif, car je veux éviter toute interruption lorsque je suis dans Neovim.

# Correction de grammaire

Pour faire une première relecture j'utilise la suite [LanguageTool](https://www.languagetool.org/).
Cette suite est disponible gratuitement et est disponible hors-ligne lorsque l'on utilise l'outil open source de correction (c'est la raison principale de mon utilisation).
Dans cet article je vais vous aider à l'installer et vous aider à l'utiliser avec Neovim.

## Installation de LanguageTool installation

LanguageTool nécessite un environnement java pour fonctionner.
Ici je choisis OpenJDK comme environnement java et j'installe également curl pour installer LanguageTool.

### Pour Fedora
```bash
sudo dnf -y install java curl
```

### Pour Ubuntu
```bash
sudo apt -y install default-jre curl
```

### Ensuite, sur les deux distributions
Les commandes qui suivent télécharge LanguageTool et l'installe dans `/usr/local`:

```bash
curl https://languagetool.org/download/LanguageTool-stable.zip > /tmp/LanguageTool-stable.zip
sudo unzip /tmp/LanguageTool-stable.zip -d /usr/local/LanguageTool
sudo mv /usr/local/LanguageTool/LanguageTool-*/* /usr/local/LanguageTool/
rm /tmp/LanguageTool-stable.zip
```


## Configuration de Neovim
J'utilise le manager de plugin [vim-plug](https://github.com/junegunn/vim-plug) pour Neovim.
C'est pourquoi je vais utiliser ce manager pour installer [le plugin de vigoux](https://github.com/vigoux/LanguageTool.nvim) (l'utilisation d'autres manager est possible).
Avant de continuer, je vous recommande d'utiliser une version récente de Neovim (&ge; 0.4.2) pour utiliser ce plugin (puisqu'il utilise des changements récents fait dans Neovim).

Dans votre `~/.config/nvim/init.vim` entre l'appel `call plug#begin(<plugging_folderpath>)` et l'appel `call plug#end()` vous devez rajouter les lignes suivantes:

```
" Language tool integration
Plug 'vigoux/LanguageTool.nvim'                                                                                 
```

Ensuite, en dehors des appels à vim-plug vous devez ajouter les lignes suivantes:

```
" grammar checking
autocmd Filetype markdown LanguageToolSetUp
autocmd Filetype tex LanguageToolSetUp
let g:languagetool_server_jar='/usr/local/LanguageTool/languagetool-server.jar'
```

## Utilisation

Pour utiliser ce plugin, vous devez faire un appel à lui dans vos documents en mode normal grâce à la commande `:LanguageToolSetUp` (cette commande est lancée automatiquement pour les fichiers tex et markdown).

Ensuite, le plugin propose les commandes suivantes:

* `:LanguageToolCheck`: pour surligner les erreurs détectées, vous devez réutiliser cette commande lorsque vous changer votre code/texte.
* `:LanguageToolSummary`: pour afficher des détails sur les mots surlignés dans une sous-fenêtre. 
* `:LanguageToolClear`: pour enlever les affichages de LanguageTool.

Pour plus d'options allez sur la [page du plugin](https://github.com/vigoux/LanguageTool.nvim).

J’espère que cela aidera certains d’entre vous.
Si vous avez une meilleure configuration ou voulez améliorer la mienne n'hésitez pas à contribuer.

À la revoyure, Vincent.