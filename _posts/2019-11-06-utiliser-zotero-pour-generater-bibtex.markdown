---
layout: post
comments: true
title:  "Utiliser Zotero pour générer des fichiers BibTeX pour vos papiers"
ref: using_zotero
date:   2019-11-06 12:00:00 +0200
categories: astuces
lang: fr
---

# Qu'est que Zotero?

[Zotero](https://www.zotero.org/) est un logiciel permettant de collecter, organiser, partager et synchroniser des papiers de recherche (ou d'autres contenus de recherche).
Je préfère Zotero à d'autres solutions, car mes besoins sont remplis, Zotero est open source et est développé par une organisation indépendante, à but non lucratif et respectant la vie privée.

# Installation

Zotero n'est pas disponible dans les dépôts d'Ubuntu.
Pour installer Zotero, j'utilise [Flatpak](https://www.flatpak.org/) car cela permet la mise à jour automatique d'autres applications dans Ubuntu.

J'utilise le dépôt [Flathub](https://flathub.org/) avec flatpak, pour le mettre en place vous devez taper:

```bash
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

Maintenant, installons Zotero:
```bash
flatpak install flathub org.zotero.Zotero
```

Zotero est maintenant installé et un raccourci est disponible dans votre environnement de travail.

## Extensions

Pour être capable d'utiliser toutes les fonctionnalités présentées dans cet article de blogue, vous devez installer les extensions suivantes:

* L'extension Zotero pour votre navigateur web.

  Pour l'installer, allez sur la page d'extension pour [Chrome](https://chrome.google.com/webstore/detail/ekhagklcjbdpajgpjgmbionohlpdbjgc) (compatible avec les navigateurs basés sur chromium) ou pour [Firefox](https://www.zotero.org/download/connectors).

* Zotero possède un système d'extensions, j'en utilise une en particulier: Better BibTeX. 

  Pour l'installer il vous faut:
  1. Télécharger le [dernier fichier xpi](https://github.com/retorquere/zotero-better-bibtex/releases/latest).
  2. Ouvrir Zotero, aller dans `Tools` -> `Add-ons`, cliquer sur la roue d'engrenage -> `Install Add-on From File...` et sélectionner le fichier d'extension que vous avez précédamment sélectionné.

# Ajouter un papier dans votre collection

Avant tout, je vous recommande de créer un compte sur le site de Zotero.
Cela vous aidera à synchroniser votre librairie sur tous vos appareils (dans mon cas mon ordinateur personnel et professionnel).
Pour vous connecter dans l'application, aller dans `Edit` -> `Preferences` -> l'onglet `Sync`. 

Maintenant vous pouvez utiliser votre navigateur internet pour chercher vos papiers sur le net (sur [arXiv](https://arxiv.org/) par exemple).
Lorsque vous voulez ajouter un papier dans votre collection, cliquer sur l'icône du connecteur Zotero dans votre navigateur web pendant que vous visualisez le PDF désiré.
Cela va automatiquement télécharger le papier pour une utilisation hors-ligne et extraire toutes les informations requises pour citer ce papier (Titre, auteurs, date...)
Le téléchargement d'informations peut être modifié à tout moment si quelque chose ne conviens pas. 

# Générer votre fichier BibTeX utilisant Zotero

Avant de générer votre fichier BibTeX, vous pouvez personnaliser le format des clés de citations que vous utiliserez dans votre fichier TeX.
Pour effectuer cette personnalisation, allez dans le menu BibTeX: Edit -> Preferences -> l'onglet Better BibTeX.
J'utilise le format par défaut des clés (car il me convient): `[auth:lower][shorttitle3_3][year]`.
Cela correspond au premier auteur (en minuscule), suivis du titre de l'article (tronqué) et l'année de publication.
Si vous voulez modifier ce format, je vous conseille de faire un tour sur la documentation officielle se trouvant [ici](https://retorque.re/zotero-better-bibtex/citing/).

Après avoir fait cela, n'oubliez pas de rafraichir les clés de citations pour chaque élément de votre collection (ce n'est pas fait automatiquement, mais la combinaison de touches Ctrl+A est votre amie).
Même si vous voulez garder le format par défaut des clés de citations de Better BibTeX, vous devez rafraichir ces clés.
Du moins une fois après l'installation de l'extension, sinon Zotero gardera son format par défaut avec des underscores (qu'il vaut mieux éviter avec LaTeX).

![rafraichir les clés](/assets/images/zotero/refresh_key.png)

Pour générer le fichier BibTeX:

1. Clic droit sur la collection (ou libraire) contenant tous les papiers que vous voulez dans votre fichier Bib puis cliquez sur `Export Collection`, comme suivant:

   ![exporter un fichier BibTeX 1/2](/assets/images/zotero/export_bibtex_1.png)

2. Dans le menu contextuel, sélectionnez le format "Better BibTeX" (qui utilise un format plus épuré que le format par défaut) et cochez la case "Keep updated" pour laisser Zotero mettre à jour le fichier résultant lorsque qu'une modification/ajout est faite sur votre librairie.

   ![exporter un fichier BibTeX 2/2](/assets/images/zotero/export_bibtex_2.png)

Vous avez maintenant votre fichier Bib que vous pouvez utiliser pour vos papiers avec un format de citation vous convenant.

J’espère que cela aidera certains d’entre vous.

À la revoyure, Vincent.
