---
layout: post
comments: true
title:  "Hackaviz 2021"
ref: hackaviz-2021
date:   2021-09-19 08:00:00 +0200
categories: blogue datavis
category: blogue
lang: fr
---

Dans ce nouvel article de blog, je vais vous présenter ma participation à l'[Hackaviz 2021](https://github.com/ToulouseDataViz/Hackaviz2021).
L'Hackaviz est un concours organisé par l'association de visualisation de données nommée [Toulouse dataviz](https://toulouse-dataviz.fr/). Elle consiste à raconter une histoire en utilisant des données Toulousaines mises à disposition.

M'amusant à faire des visualisations de données sur mon temps libre, j'y ai donc participé en solo.
Le code que j'ai utilisé est disponible [ici](https://github.com/vroger11/hackaviz2021).
Dans cet article de blog, je vais vous révéler l'histoire que j'ai souhaité raconter à travers mon travail.

# Résultats

La région Occitanie est une région bien ensoleillée avec son chef-lieu étant à Toulouse.
On s'attend donc à ce que la ville soit la plus dynamique en termes de transactions foncières entre les années 2016 et 2020.

Voyons cela au travers de ma réalisation :

{% include hackaviz2021/fr_result.html %}

Étant une figure interactive n'hésitez pas à passer vos souris sur les régions colorées pour avoir plus de détails (le nombre de transactions par type et la valeur moyenne de ces transactions).

Ainsi, Toulouse n'est pas la ville (parmi les villes dont les données sont disponibles) ayant le plus de transactions foncières. Montpellier (chef-lieu de l'ancienne région Languedoc-Roussillon) et Nîmes sont légèrement devant (2332 pour Toulouse vs 2481 pour Montpellier vs 2729 pour Nîmes). Tandis que Béziers avec ses 4251 transactions est largement devant les autres villes.
En regardant plus en détail ce graphique, on s'aperçoit qu'excepté Toulouse, les villes les plus dynamiques foncièrement sont les villes proches de la mer Méditerranée. La valeur moyenne des transactions sur cette zone est globalement plus élevé que pour les villes moins dynamiques foncièrement.

Ainsi, le bassin méditerranéen est une zone d'attraction pour les activités foncières.


J’espère que cela aidera/inspirera certains d’entre vous.
Dans tous les cas, j'espère que cela a été intéressant.

À la revoyure, Vincent.
