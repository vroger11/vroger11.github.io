---
comments: true
title:  "Hackaviz 2022"
date:   2022-04-03
tags:
    - Archives
    - Visualisations
hide:
    - navigation
excerpt: Ma seconde participation à l'évènement hackaviz proposé par l'association Toulouse dataviz.
---

Dans ce nouvel article de blog, je vais vous présenter ma participation à l'[Hackaviz 2022](https://github.com/ToulouseDataViz/Hackaviz2022).
L'Hackaviz est un concours organisé par l'association de visualisation de données nommée [Toulouse dataviz](https://toulouse-dataviz.fr/). Elle consiste à raconter une histoire en utilisant des données mises à disposition.

M'amusant à faire des visualisations de données sur mon temps libre, j'y ai donc participé en solo.
Vous trouverez ma participation à l'année précédente [ici](../2021/09-19-hackaviz.md).
Le code que j'ai utilisé sera disponible d'ici peu.

## Résultats

Cette année, une base de données des financements collectés et
distribués par les organismes de gestion collective des droits
d’auteur, au titre de la rémunération pour copie privée est à l'honneur.

J'ai consacré mon travail sur le type d'aides accordées, toutes organisations confondues.
Ainsi j'ai réalisé un premier graphique (il est interactif vous pouvez utiliser votre souris ou doigt) montrant l'évolution du nombre d'aides accordées par type:

--8<--
assets/interactive_dataviz/hackaviz2022/fr_bars.html
--8<--

On remarque ici l'effet de la covid car en 2020 il y a eu une baisse du nombre d'aides accordées (qui étaient à la hausse pour tous les types chaque année jusque là).
Avec les spectacles vivants souffrent le plus, ceci est certainement lié aux confinements.
Suite à ce premier constat, j'ai voulu observer la répartition du nombre d'aides avec le graphique suivant (lui aussi interactif):

--8<--
assets/interactive_dataviz/hackaviz2022/fr_areas.html
--8<--

Ainsi avec ce graphique on se rend compte que la répartition des financements est restée stable de 2016 à 2019 (indépendamment de l'augmentation du nombre d'aides allouées).
Néanmoins pour l'année 2020 cette répartition a changé en la défaveur des diffusions de spectacle vivant et en faveur des financements de créations. Tandis que la proportion de financements pour l'éducation artistique et culturelle et la formation des artistes est restée stable.

J’espère que cela aidera/inspirera certains d’entre vous.
Dans tous les cas, j'espère que cela a été intéressant.

À bientôt, Vincent.
