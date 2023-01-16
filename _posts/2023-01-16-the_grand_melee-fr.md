---
layout: post
comments: true
title:  "Mes visualisations du tournoi The Grand Melee"
ref: tgm1
date:   2023-01-16 12:00:00 +0200
categories: blogue datavis
category: blogue
lang: fr
---

Ce billet est le troisième épisode de ma série de tournois Age of Empires 2 (jetez un coup d'œil à [mon premier](/blogue/datavis/aoe/2022/12/08/redbull-wololo-legacy-fr) et à mon [second](/blogue/datavis/2022/12/13/warlords-fr) si vous les avez manqués).
Tout le travail effectué ici concerne l'événement principal (c'est ce qui m'intéresse le plus dans les tournois).
Pour rappel, je publie la majorité de ces visualisations sur [Reddit](https://www.reddit.com/user/vroger11) avant de réaliser ce post. Ce dernier est un résumé du tournoi et des retours que j'ai pu avoir.

Cette fois, j'ai amélioré mes visualisations grâce à des données supplémentaires venant d'[Age of Empires Captains Mode](https://aoe2cm.net/) lorsque c'était utile et en ajoutant un QR code me servant de signature pour mes œuvres.

Regardons maintenant ce travail 😃.

# Réalisations des joueurs
Ma première visualisation est celle montrant chaque rang obtenu des joueurs avec le nombre de matchs qu'ils ont gagné, perdu.
Ici, il y avait moins de joueurs pour l'évènement principal, mais malgré tout nous avons pu assister à de beaux matchs.

{:refdef : style="text-align : center ;"}
![Réalisations des joueurs](/assets/images/dataviz/aoe/tgm/1/rank_games.png){:width="1200px"}
{: refdef}

# Quelle civilisation a été jouée sur chaque carte ?

Ensuite, nous avons un sankey sur quelle civilisation a été jouée pour chaque carte.
Dans ce tournoi, les civilisations qui n'ont pas été jouées sont : Celtes, Chinois, Hongrois, Malais, Maliens, Siciliens, Slaves, Espagnols, Teutons, Vietnamiens et Vikings.
J'ai préféré le sankey à la matrice de chaleur, car la matrice possède beaucoup de zéros. À voir si les sankey sont plus lisibles sur des évènements avec plus de matchs 😉

{:refdef : style="text-align : center ;"}
![Quelles sont les civilisations jouées sur chaque carte ?](/assets/images/dataviz/aoe/tgm/1/civ_v_map_sankey.png){:width="1100px"}
{: refdef}

# Quelles cartes ont été populaires ?

Ici, j'ai rajouté les informations de ban venant des drafts des joueurs.
Ceci a plu, mais j'aimerais trouver un affichage plus adapté pour la suite.

{:refdef : style="text-align : center ;"}
![Quelles sont les cartes les plus populaires ?](/assets/images/dataviz/aoe/tgm/1/maps_played.png){:width="1200px"}
{: refdef}

# Tuer/mourir de chaque civilisation jouée

Ici, j'ai rajouté les informations de snipe et ban venant des drafts des joueurs.
J'aime toujours autant les discussions que ce graphe génère 😄.
J'ai eu quelques pistes pour améliorer ce graphe (dont avoir les snipe/bans/mort comme étant des valeurs négatives), nous verrons au prochain tournois si j'obtiens de meilleures visualisations.

{:refdef: style="text-align: center;"}
![Tué/mort pour chaque civilisation jouée](/assets/images/dataviz/aoe/tgm/1/civ_played.png){:width="1200px"}
{: refdef}

# La roue de la victoire

Enfin, la roue de la victoire !
Comme mon précédent post, c'est une exclusivité pour ceux qui lisent ce post 😉.
Vous pouvez survoler chaque segment avec votre souris pour recueillir des informations, et même cliquer sur les catégories pour examiner une carte ou une catégorie de carte spécifique (cliquez sur le logo du tournoi pour revenir en arrière).
N'hésitez pas à me donner des conseils pour améliorer ce travail.


<div style="width : 1000px ; margin : 0 auto ;">
    {% include interactive_dataviz/aoe/tgm/1/wheel.html %}
</div>

# Quelle est la prochaine étape ?

Je pense peaufiner et retravailler certains de mes graphes pour la suite.
Vous verrez cela pour le prochain tournoi que je couvrirai.
D'ailleurs, ce sera le T90 Titans League 2: Platinum League, l'hype est là !

J'espère que cela aidera et/ou inspirera certains d'entre vous.

À la revoyure, Vincent.
