---
layout: post
comments: true
title:  "Mes visualisations du tournois Warlords"
ref: warlords1
date:   2022-12-13 08:00:00 +0200
categories: blogue datavis
category: blogue
lang: fr
---

Ce billet est le second épisode de ma série de tournois Age of Empires 2 (jetez un coup d'œil à [mon premier](/blogue/datavis/aoe/2022/12/08/redbull-wololo-legacy-fr) si vous l'avez manqué).
Tout le travail effectué ici concerne l'événement principal (c'est ce qui m'intéresse le plus dans les tournois).

Ici, les deux couleurs principales que j'ai choisies dans le logo donnent des visualisations plus agréables (du moins à mon avis).
Cette fois, j'ai amélioré mes visualisations sur les aspects suivants :
- utiliser une meilleure police de caractères (ici Nimbus Sans) ;
- éviter les titres dans mes graphiques, comme nous les avons dans le post Reddit et sur ce blog ;
- intégrer le logo du tournoi dans chaque visualisation :
- intégrer des informations sur les données utilisées (ici je n'ai utilisé que les données de Liquipedia, mais je pourrais ajouter d'autres sources dans les tournois futurs) ;
- augmenter la taille de la police pour être plus facile à lire (peut-être pas assez pour les smartphones, cependant).

Assez parlé, regardons maintenant mon travail 😃.

# Réalisations des joueurs
Ma première visualisation était celle qui montrait les réalisations de chaque joueur concernant leur rang à côté du nombre de matchs qu'ils ont gagné, perdu.
Ici, Classic Pro a dû déclarer forfait et Jordan a eu une victoire automatique.
J'ai essayé des choses pour l'illustrer, mais je n'étais pas satisfait du résultat, alors j'ai fini avec le suivant :

{:refdef : style="text-align : center ;"}
![Réalisations des joueurs](/assets/images/dataviz/aoe/warlords/1/rank_games.png){:width="1200px"}
{ : refdef}

# Quelle civilisation a été jouée sur chaque carte ?

Ensuite, nous avons la matrice de quelle civilisation a été jouée pour chaque carte.
Dans ce tournoi, les civilisations qui n'ont pas été jouées sont : Goths, Slaves, Vietnamiens, Huns, Celtes et Siciliens.
Ici, nous avons une énorme matrice, car nous avons plus de cartes par rapport au Red Bull Wololo Legacy.
Cette matrice est maintenant plus simple à lire, mais l'esthétique n'est pas suffisante pour moi (peut-être que la prochaine fois sera meilleure).

{:refdef : style="text-align : center ;"}
![Quelles sont les civilisations jouées sur chaque carte ?](/assets/images/dataviz/aoe/warlords/1/map_civ_played.png){:width="1100px"}
{ : refdef}

# Quelles cartes ont été populaires ?

Cette visualisation a suscité plus d'intérêt que celle du tournoi précédent.
Je pense que c'est dû à la façon dont j'ai trié les abscisses, mais aussi parce que le pool de cartes est assez grand pour nécessiter une telle visualisation afin de mieux comprendre ce qui s'est passé.

{:refdef : style="text-align : center ;"}
![Quelles sont les cartes les plus populaires ?](/assets/images/dataviz/aoe/warlords/1/maps_played.png){:width="1200px"}
{ : refdef}

# Tuer/mourir de chaque civilisation jouée

Ici, comme pour la visualisation ci-dessus, j'ai trié les abscisses en fonction du nombre de parties jouées.
Je pense que c'est mieux que de les trier par ordre alphabétique.
Certains ont demandé le taux de victoire, si c'est compréhensible pour les civilisations jouées plus de 10 fois, je doute que ce soit bon pour l'autre cas.
J'aime toujours autant les discussions que cela génère 😄.
Malgré tout, je compte travailler sur celui-ci pour en ajoutant des informations.

{:refdef: style="text-align: center;"}
![Tué/mort pour chaque civilisation jouée](/assets/images/dataviz/aoe/warlords/1/civ_played.png){:width="1200px"}
{: refdef}

# La roue de la victoire

Enfin, la roue de la victoire !
Par rapport à la dernière fois, je n'ai pas publié de png sur Reddit.
C'est une exclusivité pour ceux qui lisent ce post 😉.
Comme je voulais qu'elle apporte des informations supplémentaires par rapport au dernier tournoi (aussi parce que le pool de cartes est énorme), j'ai opté pour une visualisation interactive.
Le résultat est moins beau que le précédent de Red Bull, mais j'espère que vous l'apprécierez.
Vous pouvez survoler chaque segment avec votre souris pour recueillir des informations, et même cliquer sur les catégories pour examiner une carte ou une catégorie de carte spécifique (cliquez sur le logo du seigneur de la guerre pour revenir en arrière).
N'hésitez pas à me donner des conseils pour améliorer ce travail.


<div style="width : 1000px ; margin : 0 auto ;">
    {% include interactive_dataviz/aoe/warlords/1/wheel.html %}
</div>

# Quelle est la prochaine étape ?

Je veux rassembler plus de données grâce à différentes sources pour compléter ou créer de nouvelles visualisations de données.
Pour l'instant, je vais me concentrer sur les données du [mode capitaine](https://www.aoe2cm.net/), et si vous connaissez des API que je peux utiliser, n'hésitez pas à me le dire dans les commentaires.
De même, si vous connaissez des contacts que je devrais atteindre, n'hésitez pas à partager 😄.

Le prochain tournoi que je couvrirai est le [Grand Mêlée](https://youtu.be/QMApAc6IojQ), l'hype est là !

J'espère que cela aidera et/ou inspirera certains d'entre vous.

À la revoyure, Vincent.
