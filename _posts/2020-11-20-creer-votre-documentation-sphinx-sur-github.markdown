---
layout: post
comments: true
title:  "Créer votre documentation sphinx sur GitHub"
ref: sphinx_doc
date:   2020-12-11 08:00:00 +0200
categories: blogue dev
category: blogue
lang: fr
---


Lors d'un précédent article de blogue, j'ai annoncé l'aperçu de ma librairie Audio Loader.
Elle prépare des batchs d'audios pour des librairies de réseaux de neurones (La librairie se trouve [ici](https://github.com/vroger11/audio_loader)).
Pour cette librairie j'avais planifié de créer un site internet de documentation.
Une version préliminaire est maintenant disponible [ici](https://website.vincent-roger.fr/audio_loader/).
Comme Audio Loader est une librairie Python, j'ai choisi [Sphinx](https://www.sphinx-doc.org/) pour générer la documentation de cette librairie.
Sphinx utilise le format de fichier reStructuredText pour créer la documentation.
Néanmoins, je préfère la syntaxe du markdown qui est plus simple et que j'utilise plus souvent.
Ainsi, j'utilise autant de fichiers markdown que possible pour créer mes documentations avec Sphinx.
Ensuite, pour héberger la documentation j'utilise GitHub Pages vu que je l'utilise déjà pour ce blogue.

Dans cet article de blogue, je vais vous expliquer comment créer une documentation en utilisant Sphinx et autant de fichiers markdown que possible.
Enfin, j'expliquerai comment mettre en place votre dépôt GitHub pour héberger cette documentation.


# Créer sa première documentation

Dans cette partie, nous allons voir comment créer une documentation en utilisant Sphinx.

## Installer Sphinx
Avant tout, il faut installer sphinx:
```bash
pip install sphinx
```

Maintenant, nous avons tout le nécessaire pour créer notre documentation :smile:.

## Créer les fichiers initiaux

Sphinx organise la documentation en 3 éléments:
- un fichier de compilation pour générer la documentation
- les fichiers sources contenant les instructions pour générer la documentation
- la documentation résultante (fichier pdf, pages html, ...)

Maintenant, créons cette organisation en utilisant l'outil `quickstart` de sphinx:
```bash
mkdir docs
cd docs
sphinx-quickstart
```

Dans les instructions de quickstart, choisissez de séparer les dossiers sources des dossiers de build/résultats (ceci facilitera les prochaines étapes).
Remplissez le reste comme bon vous semble.

Après cela, Sphinx créé deux dossiers (`docs/sources` and `docs/build`) et un fichier de compilation (`docs/Makefile`).

Les fichiers sources sphinx se trouvent dans `docs/sources` et le fichier `index.rst` représente la page de présentation de votre documentation (l'équivalent de `index.html` pour un site internet).
Pour compiler votre documentation en un site internet ou un fichier pdf, vous pouvez utiliser le fichier de compilation (en utilisant `make html` ou `make pdf`).
La documentation résultante se trouvera dans `docs/build`.

# Inclure/lier des fichiers dans un document rst

Ici je ne vais pas détailler le format rst vu que je l'évite autant que possible.
À la place, je vais vous montrer comment ajouter des documents et/ou liens vers d'autres fichiers au sein d'un fichier rst.
Néanmoins, si vous voulez apprendre le format je vous conseille d'aller [ici](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html).

Je préfère éviter un maximum le format rst vu que je peux utiliser des fichiers markdown la plupart du temps.
Néanmoins pour les cas où les fichiers rst sont nécessaires (comme `index.rst`), il est utile de savoir inclure ou faire un lien à des fichiers.
Par conséquent, c'est ce que je vais expliquer dans cette partie.

## Lier un fichier rst à l'aide du doctree
Pour rajouter un lien vers un fichier (disons `extra_document.rst`) il vous faut ajouter `extra_document` dans le doctre comme suit:

```rst
.. toctree::
    :maxdepth: 2

    extra_document
```

**Attention:**

L'indentation de `extra_document` doit être comme l'indentation de la ligne contenant `:maxdepth: 2`.
Si ce n'est pas le cas, vous aurez un avertissement vous disant que sphinx ne trouve pas le fichier (cela m'a pris des heures pour m'en rendre compte).


## Inclure un fichier rst dans un autre fichier rst

Pour ajouter le contenu d'un fichier `rst` à l'intérieur d'un autre fichier `rst`, vous devez ajouter la ligne suivante:

```bash
.. include:: my_file.rst
```

Le document contenant cette ligne sera rempli du contenu de `my_file.rst`.

# Écrire une documentation sphinx à l'aide de fichiers markdown et d'autodoc

## Configurer sphinx pour le markdown

Pour ajouter le support du markdown dans sphinx, j'utilise `m2r` au lieu de `recommonmark` (officiellement recommandé par sphinx).
La raison est simple: `recommonmark` ne supporte pas la commande `mdinclude` pour inclure les documents markdown dans des fichiers `rst`.
Maintenant, installons `m2r`:
```bash
pip install m2r
```

Ensuite, nous devons modifier le fichier `source/config.py` pour lui  ajouter cette ligne:
```python
extensions.append("m2r")
```

Maintenant, vous pouvez utiliser des fichiers markdown pour votre documentation.
Vous pouvez ajoutez ces derniers dans le doctree (comme pour les fichiers `rst`) ou les inclure en utilisant `mdinclude`.
Un petit exemple avec `mdinclude`:
```bash
.. mdinclude:: my_file.md
```

## Autodoc
Pendant la création d'une documentation, il est agréable d'utiliser la docstring des fichiers sources pour remplir la documentation.
Ici je vais expliquer comment vous pouvez utiliser l'extension `autodoc`.

### Installation
Pour activer l'extension, il faut ajouter la ligne suivante dans le fichier `source/config.py`:
```python
extensions.append('sphinx.ext.autodoc')
```

Il faut également ajouter les lignes suivantes en début du fichier `source/config.py`:
```python
import os
import sys
sys.path.insert(0, os.path.abspath('../..'))
```

Pour utiliser le format numpy dans vos docstring vous devez également ajouter l'extension napoleon dans le fichier `source/config.py`:
```python
extensions.append('sphinx.ext.napoleon')
```

Nous sommes maintenant prêts pour utiliser autodoc.

### Montrer des éléments de la docstring

Toutes les commandes qui vont suivre fonctionnent au sein de fichiers `rst` (je cherche toujours un moyen simple pour utiliser ces commandes dans un fichier markdown, n'hésitez pas à partager vos conseils en commentaire :smile:).
Pour plus de détails, suivez la [documentation officielle](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html).

#### Utiliser la docstring d'un module
```rst
.. automodule:: project_folder.module
```

#### Utiliser toutes les docstring d'un module
```rst
.. automodule:: project_folder.module
	:members:
```

#### Utiliser la docstring d'une classe
```rst
.. autoclass:: project_folder.module.class
```


# Thème sphinx

Maintenant, pensons esthétique :smile:.
Nous allons utiliser les thèmes se trouvant [ici](https://sphinx-themes.org/).

Sur le site internet, choisissez votre style désiré, puis cliquez sur l'hyperlink pypi pour installer le thème en utilisant la commande `pip`.
Pour appliquer votre thème, cliquez sur l'hyperlink `conf.py` associé et regardez la variable `html_theme`.
Ensuite copiez le code correspondant pour modifier votre variable `html_theme` dans votre fichier `source/config.py`.

# Héberger votre documentation sur GitHub

Maintenant que nous savons comment créer une documentation, il serait bien de l'héberger sur un serveur.
Ici, je vais expliquer comment j'ai fait avec les serveurs GitHub.

## Préparer la structure de votre projet
Pour mes projets, je pense que la structure la plus adaptée est la suivante:
```
my_project
|-  my_project_code      --> dossier contenant le code source et les sources de la documentation
|-  my_project_gh_pages  --> dossier contenant votre site web ou pdf
|   |-  html             --> dossier contenant votre documentation (synchronisé avec la branche gh_pages)
```

Maintenant, créons cette structure.
En premier, créons un répertoire pour votre projet:
```bash
mkdir my_project
cd my_project
```

Puis, créons les dossiers `my_project/my_project_code` et `my_project/my_project_gh_pages`:
```bash
mkdir my_project_gh_pages
git clone https://github.com/username/my_project
mv my_project my_project_code
```

Maintenant, créons et préparons le dossier `html`:
```bash
git clone https://github.com/username/my_project
mv my_project html
cd html
git branch gh-pages

git symbolic-ref HEAD refs/heads/gh-pages  # auto-switches branches to gh-pages
# supprime l'index pour la branche gh-pages
rm .git/index
git clean -fdx
```

## Modifier le makefile de sphinx
Maintenant, nous devons modifier le makefile pour utiliser la branche gh-branch et le dossier `my_project_gh_pages`.

Pour cela, éditez le fichier `my_project/my_project_code/docs/Makefile` tel que la variable `BUILDDIR` soit comme tel:
```
BUILDDIR      = ../../my_project_gh_pages/
```

Vous pouvez maintenant générer votre documentation avec la commande suivante:
```bash
make html
```

## Créer vos fichiers html pour GitHub

Votre documentation est maintenant dans le répertoire `my_project/my_project_gh_pages/html`.
Maintenant il vous reste à faire un `commit` et `push` comme suit:
```bash
cd my_project_gh_pages/
git add html
git commit -m "First documentation commit"
git push --set-upstream origin gh-pages
```


La documentation est maintenant disponible sous `https://username.github.io/my_project`, à moins que vous n'ayez fait de redirection pour votre GitHub Pages (comme pour ce blogue).

## Forcer la désactivation de jekyll

Les serveurs de GitHub utilisent jekyll et cela peut provoquer des erreurs d'interprétations de fichiers html générés par sphinx.
Heureusement, nous pouvons désactiver jekyll.

À l'intérieur du dossier `my_project/my_project_gh_pages/html` tapez les commandes suivantes:
```bash
touch .nojekyll
git add .nojekyll
git commit -m "added .nojekyll"
git push origin gh-pages
```

## Construire la documentation et la commiter automatiquement

Vous pouvez ajouter une commande dans votre Makefile:
```bash
	pushhtml: html

    cd $(BUILDDIR)/html; git add . ; git commit -m "rebuilt docs"; git push origin gh-pages
```

Maintenant, en tapant `make pushhtml` vous pouvez construire votre documentation, la commiter et la pousser automatiquement sur GitHub.
Elle reste au côté de la commande `make html` qui peut être utilisée pour tester vos documentations dans les pousser sur GitHub.

# Sources/Inspirations
* [La documentation Official de sphinx](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

* [Getting started with sphinx](https://docs.readthedocs.io/en/stable/intro/getting-started-with-sphinx.html)

* [Getting started with autodoc](https://medium.com/@eikonomega/getting-started-with-sphinx-autodoc-part-1-2cebbbca5365)

* [Publishing sphinx documentation on GitHub](https://daler.github.io/sphinxdoc-test/includeme.html)

J’espère que cela aidera certains d’entre vous.

À la revoyure, Vincent.
