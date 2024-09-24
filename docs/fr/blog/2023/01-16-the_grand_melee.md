---
comments: true
title:  "Mes visualisations du tournoi The Grand Melee"
date:   2023-01-16
tags:
    - Archives
    - Age of Empires
    - Visualisations
hide:
  - navigation
excerpt: Encore une fois, un autre tournoi d'age of empires 2, voyons mes nouvelles visualisations.
---

Ce billet est le troisième épisode de ma série de tournois Age of Empires 2 (jetez un coup d'œil à [mon premier](../2022/12-08-redbull-wololo-legacy.md) et à mon [second](../2022/12-13-warlords.md) si vous les avez manqués).
Tout le travail effectué ici concerne l'événement principal (c'est ce qui m'intéresse le plus dans les tournois).
Pour rappel, je publie la majorité de ces visualisations sur [Reddit](https://www.reddit.com/user/vroger11) avant de réaliser ce post. Ce dernier est un résumé du tournoi et des retours que j'ai pu avoir.

Cette fois, j'ai amélioré mes visualisations grâce à des données supplémentaires venant d'[Age of Empires Captains Mode](https://aoe2cm.net/) lorsque c'était utile et en ajoutant un QR code me servant de signature pour mes œuvres.

Regardons maintenant ce travail 😃.

## Réalisations des joueurs

Ma première visualisation est celle montrant chaque rang obtenu des joueurs avec le nombre de matchs qu'ils ont gagné, perdu.
Ici, il y avait moins de joueurs pour l'évènement principal, mais malgré tout nous avons pu assister à de beaux matchs.

<figure markdown="span">
![Réalisations des joueurs](../../../assets/images/dataviz/aoe/tgm/1/rank_games.png){:width="1200px"}
</figure>

## Quelle civilisation a été jouée sur chaque carte ?

Ensuite, nous avons un sankey sur quelle civilisation a été jouée pour chaque carte.
Dans ce tournoi, les civilisations qui n'ont pas été jouées sont : Celtes, Chinois, Hongrois, Malais, Maliens, Siciliens, Slaves, Espagnols, Teutons, Vietnamiens et Vikings.
J'ai préféré le sankey à la matrice de chaleur, car la matrice possède beaucoup de zéros. À voir si les sankey sont plus lisibles sur des évènements avec plus de matchs 😉

<figure markdown="span">
![Quelles sont les civilisations jouées sur chaque carte ?](../../../assets/images/dataviz/aoe/tgm/1/civ_v_map_sankey.png){:width="1100px"}
</figure>

## Quelles cartes ont été populaires ?

Ici, j'ai rajouté les informations de ban venant des drafts des joueurs.
Ceci a plu, mais j'aimerais trouver un affichage plus adapté pour la suite.

<figure markdown="span">
![Quelles sont les cartes les plus populaires ?](../../../assets/images/dataviz/aoe/tgm/1/maps_played.png){:width="1200px"}
</figure>

## Tuer/mourir de chaque civilisation jouée

Ici, j'ai rajouté les informations de snipe et ban venant des drafts des joueurs.
J'aime toujours autant les discussions que ce graphe génère 😄.
J'ai eu quelques pistes pour améliorer ce graphe (dont avoir les snipe/bans/mort comme étant des valeurs négatives), nous verrons au prochain tournois si j'obtiens de meilleures visualisations.

<figure markdown="span">
![Tué/mort pour chaque civilisation jouée](../../../assets/images/dataviz/aoe/tgm/1/civ_played.png){:width="1200px"}
</figure>

## La roue de la victoire

Enfin, la roue de la victoire !
Comme mon précédent post, c'est une exclusivité pour ceux qui lisent ce post 😉.
Vous pouvez survoler chaque segment avec votre souris pour recueillir des informations, et même cliquer sur les catégories pour examiner une carte ou une catégorie de carte spécifique (cliquez sur le logo du tournoi pour revenir en arrière).
N'hésitez pas à me donner des conseils pour améliorer ce travail.

--8<--
assets/interactive_dataviz/aoe/tgm/1/wheel.html
--8<--

J'espère que cela aidera et/ou inspirera certains d'entre vous.

À bientôt, Vincent.
