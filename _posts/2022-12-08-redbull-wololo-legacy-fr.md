---
layout: post
comments: true
title:  "Mes visualisations du tournoi Red Bull Wololo legacy"
ref: rbw6
date:   2022-12-08 08:00:00 +0200
categories: blogue datavis aoe
category: blogue
lang: fr
---

Pendant mon temps libre, je joue à Age of Empires 2 (le jeu vidéo de mon enfance).
J'aime aussi regarder des tournois avec des joueurs professionnels.
Récemment, il y a eu le plus grand tournoi d'Age of Empires 2 organisé par Red Bull : [Red Bull Wololo Legacy](https://www.redbull.com/int-en/events/red-bull-wololo-legacy/).
Ce tournoi comprenait une cagnotte de 200 000 $ !
Plutôt bien pour un jeu vieux de 25 ans.

Comme vous pouvez le voir sur mon blog (également sur Twitter et Reddit), j'aime publier des visualisations de données.
Aussi, j'ai intégré une association de data visualisation (Toulouse dataviz).
Pour me perfectionner en data visualisation, pourquoi ne pas fusionner ces deux passe-temps ?

Nous y voilà, dans ce post, je montrerai toutes les visualisations de données que j'ai faites pour ce Tournoi.
Dans ce post, vous avez les graphiques améliorés (principalement des changements esthétiques) comparés à ceux que j'ai publiés sur Twitter et Reddit.
Pour réaliser ces graphiques, j'ai utilisé les données de [Liquipedia](https://liquipedia.net/ageofempires/Main_Page), généré des graphiques avec [Plotly](https://plotly.com/) et les ai affinés (avec des logos, des fonds sur les figures...) avec [Inkscape](https://inkscape.org/fr/).
J'ai choisi d'utiliser les deux couleurs principales du logo Red Bull pour mes créations, car à l'époque, je pensais que cela pourrait être intéressant de créer une sorte de série.

Je suis heureux que cela ait si bien fonctionné, ce billet est donc le début d'une série de billets sur les dataviz pour les tournois Age of Empires 2 !

Assez parlé, regardons maintenant mon travail 😃.

# Réalisations des joueurs
Ma première dataviz était de montrer les réalisations de chaque joueur concernant leurs classements ainsi que le nombre de parties qu'ils ont gagnées, perdues.
Elle inclut également le prix qu'ils ont gagné. Je suis assez content de celle-ci, car elle parait agréable et possède de multiples informations.

{:refdef: style="text-align: center;"}
![Réalisations des joueurs](/assets/images/dataviz/aoe/rbw/6/rank_games.png){:width="1200px"}
{: refdef}

# Matchs de civilisations

Ensuite, nous avons la dataviz avec le plus de retours (positifs et négatifs).
Dans ce travail, j'ai créé une matrice de confusion et comme elles sont symétriques, j'ai supprimé la redondance (une habitude comme gars de l'apprentissage automatique 😅).
Je trouve le résultat agréable, mais je ne suis pas convaincu de la lisibilité (j'ai reçu quelques commentaires sur ce point).
Comme il a nécessité un effort important dans Inkscape, il est possible que je n'inclue pas celui-ci (tel quel ou pas du tout) pour les prochains tournois.

{:refdef: style="text-align: center;"}
![Civilizations matchups](/assets/images/dataviz/aoe/rbw/6/civ_vs_civ_played.png){:width="1000px"}
{: refdef}

# Quelle civilisation a été jouée sur chaque carte ?

Ensuite, nous avons une matrice de quelle civilisation a été jouée pour chaque carte.
Simple et efficace, j'aime le contenu, mais peu l'esthétique.
Je vais essayer d'améliorer cela pour les prochains tournois.

{:refdef: style="text-align: center;"}
![Quelles civilisations sont jouées sur chaque carte ?](/assets/images/dataviz/aoe/rbw/6/map_civ_played.png){:width="1200px"}
{: refdef}

# Le ratio Kill/death de chaque civilisation jouée

Celui qui a créé de nombreuses discussions sur les stratégies et les équilibres des civilisations.
J'adore discuter et voir les explications des autres, ça m'aide comme joueur, et c'est toujours amusant 😄.

{:refdef: style="text-align: center;"}
![Ratio morts/morts de chaque civilisation jouée](/assets/images/dataviz/aoe/rbw/6/civ_played.png){:width="1200px"}
{: refdef}

# La roue de la victoire

Enfin, mon dernier travail, une roue des civilisations victorieuses sur chaque carte.
Je l'ai fait sous forme de roue pour éviter les blancs d'un équivalent utilisant des matrices.
Le résultat est une création esthétique (je suis partial, car je suis content de l'aspect) et informative.
Elle comprend les proportions de parties sur chaque carte et les proportions de victoire des civilisations sur chaque carte.

Ce graphique demandera du travail pour les prochains tournois (pour que les personnes voient les chiffres de même que les proportions), mais c'était amusant à faire.

{:refdef: style="text-align: center;"}
![La roue de la victoire](/assets/images/dataviz/aoe/rbw/6/wheel.png){:width="1000px"}
{: refdef}

# Quelle est la suite ?

Pour cet événement, j'ai publié simultanément sur Twitter et Reddit.
Mes créations ont obtenu plus de vues et de réactions sur Reddit.
À titre de comparaison, ma première visualisation a été vue 30 fois sur Twitter (sans interactions) et plus de 35 000 sur Reddit !
Sur Reddit, ces dataviz ont généré des discussions sur le tournoi. J'ai aussi reçu des retours avec de bons conseils pour améliorer mon travail (ce qui sera vu pour les prochains tournois).

Si vous ne l'avez pas encore vu, le prochain tournoi que je couvrirai sera [Warlords] (https://youtu.be/OhXuB5w-pS8).

J'espère que cela aidera et/ou inspirera certains d'entre vous.

À la revoyure, Vincent.
