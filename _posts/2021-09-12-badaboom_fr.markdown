---
layout: post
comments: true
title:  "BadaBoom"
ref: badaboom
date:   2021-09-12 08:00:00 +0200
categories: blogue dev
category: blogue
lang: fr
---

Dans ce nouvel article de blog, je vais vous présenter un petit projet que j'ai réalisé dernièrement.
Étant passionné d'espace, j'ai cherché une base de données simple qui me permettrait d'appréhender l'immensité du monde qui nous entoure.
Je suis finalement tombé sur la base 'Asteroids - NeoWs' fournie par la NASA (pour plus d'informations, c'est par [ici](https://api.nasa.gov/)).
Cette base est focalisée sur les astéroïdes passant près de la planète Terre.
Dans cette base, le point le plus proche d'un astéroïde passant auprès de la Terre représente un évènement.

J'ai donc réalisé un dépôt nommé [badaboom](https://github.com/vroger11/badaboom). Ce dernier contient un parseur (extracteur de données) et un programme permettant de réaliser quelques figures et statistiques.
Dans cet article de blog, je vais détailler les premiers résultats que j'ai obtenus (et mettrais à jour ce post en fonction de mes avancées).

# Résultats
Tous les résultats qui suivront sont calculés pour les années comprises entre 1980 et 2030. Sachant que les données étaient récupérées le 10 septembre 2021, toutes les données de cette date à fin 2030 sont les projections de la NASA.


Avant de commencer, voici les 10 plus grands astéroïdes pour les années entre 1980 et 2030:

|   Identifiant de l'astéroïde | Nom de l'astéroïde               |   Diamètre minimum estimé (km) |   Diamètre maximum estimé (km) |   [Amplitude absolue h](https://ssd.jpl.nasa.gov/?glossary&term=H) | Lien d'information de l'astéroïde                                         |
|:----------------------------|:----------------------------|-------------------------:|-------------------------:|-----------------------:|:----------------------------------------------|
|                     2001036 | 1036 Ganymed (A924 UB)      |                 37.5452  |                  83.9537 |                   9.25 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2001036> |
|                     2000433 | 433 Eros (A898 PA)          |                 21.8049  |                  48.7573 |                  10.43 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2000433> |
|                     2001866 | 1866 Sisyphus (1972 XA)     |                  8.64082 |                  19.3215 |                  12.44 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2001866> |
|                     2004954 | 4954 Eric (1990 SQ)         |                  8.06408 |                  18.0318 |                  12.59 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2004954> |
|                     2001627 | 1627 Ivar (1929 SH)         |                  7.7724  |                  17.3796 |                  12.67 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2001627> |
|                     2003552 | 3552 Don Quixote (1983 SA)  |                  6.80072 |                  15.2069 |                  12.96 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2003552> |
|                     2002212 | 2212 Hephaistos (1978 SB)   |                  5.73528 |                  12.8245 |                  13.33 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2002212> |
|                     2025916 | 25916 (2001 CP44)           |                  4.88152 |                  10.9154 |                  13.68 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2025916> |
|                     2001980 | 1980 Tezcatlipoca (1950 LA) |                  4.61907 |                  10.3286 |                  13.8  | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2001980> |
|                     2005587 | 5587 (1990 SB)              |                  4.57673 |                  10.2339 |                  13.82 | <http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2005587> |

Ainsi, on s'aperçoit que des astéroïdes gigantesques passent au-dessus notre tête sans même que l'on ne s'en rende compte !
Certains d'entre vous remarqueront que certains noms d'astéroïdes leur sont familiers (surtout si vous regardez la série The Expanse).

Maintenant regardons le nombre d'astéroïdes nous visitant par mois (la figure est interactive n'hésitez pas à passer votre souris sur les barres):

{% include badaboom/Fig.1-unique_events_per_year.html %}

En voyant ce graphique on peut en tirer les points suivants:
- Plus les années passent et plus on observe d'astéroïdes (surement lié à une amélioration de nos observatoires, même si plus d'astéroïdes peuvent nous croiser ces dernières années).
- Le nombre de passages prévus d'astéroïdes est plus faible que ces 5 dernières années. Cela est peut-être dû au fait que les efforts sont mis sur les astéroïdes les plus gros (voir la figure suivante qui peut laisser penser cette ceci).
- Les astéroïdes ne font en général pas plus d'un passage par an.


Allons maintenant plus en détail avec la figure suivante (elle aussi interactive):

{% include badaboom/Fig.2-asteroid_size_per_year.html %}

Les catégories d'astéroïdes sont complètements arbitraires. C'est surtout que j'arrive à me visualiser ces différentes tailles avec ces catégories.
On remarque que l'on détecte de plus en plus d'astéroïdes dont la taille est inférieure à 500m. Tandis que lorsque l'on regarde les prévisions on s'aperçoit que le nombre prévu est bien moindre. Surement parce que les plus gros astéroïdes sont prioritaires pour la surveillance autour de notre planète.

En addition à ce graphique, j'ai calculé les nombres suivants:

- Astéroïde unique rencontré et observé de 1980 à 2021-09-10 : 25810
- Astéroïde unique dont la rencontre est prévue entre 2021-09-10 et 2030 : 11211

Il y a malgré tout pas mal de visites de prévues, en espérant que les plus gros n'entrent pas en contact (pour les plus anxieux, ce n'est pas au programme).

Voilà, c'est tout pour aujourd'hui, j'ai réalisé ce projet pour ma curiosité et je me suis dit que je pouvais vous le partager. J'essaierai de mettre à jour ce post annuellement et créerai d'autres posts dans ce style à l'avenir.


J’espère que cela aidera/inspirera certains d’entre vous.
Dans tous les cas, j'espère que cela a été intéressant.

À la revoyure, Vincent.
