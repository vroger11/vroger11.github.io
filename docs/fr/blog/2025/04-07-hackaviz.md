---
comments: true
title:  "Ma participation à l'Hackaviz"
date:   2025-04-07
tags:
    - Développement
    - Visualisation de Données
    - Python
hide:
    - navigation
excerpt: Ma participation à la compétition Hackaviz 2025.
---

Hackaviz est un concours annuel de datavisualisation basé sur des données ouvertes, organisé par **Toulouse DataViz**.
J'ai participé à l'édition 2025 de cette compétition.
À partir des données fournies, j'ai créé un outil interactif pour explorer les niveaux d'eau et les précipitations dans la région toulousaine.
Voici un aperçu des étapes que j'ai suivies pour construire cette visualisation.

## 🎯 Mes objectifs

Avant de commencer, j'ai défini mes objectifs :

- créer une interaction entre au moins deux graphiques (je voulais apprendre cette compétence en expérimentant avec Plotly et Streamlit)
- prendre du plaisir dans le processus
- soumettre une solution aboutie et bien pensée

## 🔍 Phase d'exploration

Comme pour la plupart des projets data, tout a commencé par quelques étapes simples mais essentielles pour mieux comprendre les données :

- L’exécution de `df.describe()` sur les variables m’a permis d’identifier des valeurs min/max étranges et de fortes variances.
- Des graphiques linéaires simples par date ont révélé des tendances, des trous dans les données, et quelques valeurs aberrantes.
- Ces premières étapes m'ont permis de **détecter les données manquantes**, de normaliser les valeurs des stations et de préparer une exploration plus guidée.

## 🚀 L’application finale

Mon application s'appelle **Toulouse Water Level & Rainfall Explorer**. Son objectif est d’aider les utilisateurs à comprendre les interactions entre niveaux d’eau et précipitations à Toulouse, à partir de données ouvertes réelles. Voici quelques liens vers ma réalisation :

- [Application en ligne](https://vroger11-hackaviz-2025.streamlit.app/)
- [Code source](https://github.com/vroger11/hackaviz-2025)
- [Vidéo de démonstration](https://www.youtube.com/watch?v=2wekZIu_DFI)

La visualisation est composée de deux graphiques : une courbe des niveaux d'eau et une carte des précipitations associées.
Avec le premier graphique, vous pouvez sélectionner une plage de points pour filtrer les dates utilisées dans la carte des précipitations.
Vous pouvez également utiliser la barre latérale pour filtrer les dates des observations du niveau d’eau à Toulouse et ajuster le nombre de stations de pluie affichées sur la carte.

## 🧠 Retours & apprentissages

Ce dont je suis content :

- avoir évité les erreurs classiques liées aux valeurs aberrantes
- avoir livré une application fonctionnelle et interactive
- mon application peut gérer un grand nombre de points (près de 150 ans de données peuvent être affichés sur le premier graphique)
- avoir appris à créer une interaction entre deux graphiques Plotly dans une app Streamlit (objectif personnel atteint)

Pistes d'amélioration :

- améliorer l’aspect visuel de mes visualisations (en utilisant de meilleurs éléments graphiques et un design plus soigné)
- comme de nombreuses dates sont manquantes pour les précipitations, je pourrais colorer les points du premier graphique en bleu lorsqu’une donnée de pluie est disponible, et en gris sinon (plutôt qu’en fonction de l’accélération)
- la création d'une version multi-langue

Un grand merci à l’association **Toulouse DataViz** pour l’organisation de Hackaviz 2025 et la mise à disposition de jeux de données aussi riches.
Hackaviz a été une excellente opportunité pour travailler sur des données réelles et explorer de nouvelles idées de visualisation.

---

*Dites-moi ce que vous en pensez, j’espère que ça vous a été utile.*
À bientôt,
Vincent
