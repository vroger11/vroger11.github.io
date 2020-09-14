---
layout: post
comments: true
title:  "Audio loader - Une librairie pour charger les caractéristiques audios et vérité terrain pour les librairies de Deep Learning"
ref: audio_loader
date:   2020-03-27 12:00:00 +0200
categories: projets
lang: fr
---

J'ai publié une version anticipée d'une des librairies sur lesquelles je travaille.
Elle est appelée [Audio Loader](https://github.com/vroger11/audio_loader) et est conçue pour charger des batchs d'audios (avec caractéristiques et vérité terrain) pour des librairies de Deep Learning comme PyTorch ou TensorFlow.

Pour l'instant, c'est les premiers travaux et seulement des mécanismes basiques sont fournis, tel que:
* Des liens vers PyTorch (basiques qui remplissent toutes les données en RAM et conçus pour des sampleurs fenêtrés)
* Des chargeurs de vérité terrain génériques, avec un exemple sur TIMIT et le corpus C2SI.
* Des sampleurs génériques (chargement utilisant des fenêtres ou utilisant les fichiers en entier).
* Calculs de caractéristiques basique utilisant la librairie [librosa](https://librosa.github.io/librosa/) (seulement les MFCC et l'audio brut sont implémentés).
* Détection d'activités vocales basiques pour filtrer les signaux utilisés.
* La plupart des fonctionnalités sont testées à l'aide de [pytest](https://docs.pytest.org).

Ma liste de tâches pour ce projet (non exhaustive):
1. Ajouter un site internet pour la documentation.
2. Ajouter d'autres caractéristiques audios (spectrogrammes et autres).
3. Avoir un système élégant de cache, comme alternative à remplir tout en RAM.
4. Liens vers TensorFlow.

Pour plus de détails, allez dans le [répertoire GitHub](https://github.com/vroger11/audio_loader) (tout est en anglais).
Si vous voulez contribuer, n'hésitez pas à me contacter ou à proposer vos pull request :smile:.

J’espère que cela aidera certains d’entre vous.

À la revoyure, Vincent.
