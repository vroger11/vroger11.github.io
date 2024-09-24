---
comments: true
title:  "Badaboom - Les boules de feu"
date:   2021-12-15
tags:
    - Archives
    - Visualisations
hide:
    - navigation
excerpt: Visualisations réalisées pour explorer les données de CNEOS (projet appartenant à la NASA).
---

Dans ce nouvel article de blog, je vais vous présenter la suite de mes travaux sur ma librairie [badaboom](https://github.com/vroger11/badaboom)).
Cet article fait suite à mon précédent article sur mon projet badaboom (vous pouvez retrouver cet article [ici](09-12-badaboom-asteroids.md)).
Pour ce deuxième volet, j'ai réalisé l'analyse des boules de feu (fireballs en anglais).
Les boules de feu sont des termes astronomiques désignant des météores exceptionnellement brillants et suffisamment spectaculaires pour être vus dans une zone très étendue.

Pour ce travail, j'ai utilisé la base de données du CNEOS (appartenant à la NASA).
Je vais pour cet article, commencer par vous présenter les éléments présents dans cette base de données et finir par vous montrer les premiers résultats que j'ai obtenus (je mettrais à jour ce post compte tenu de mes avancées).

!!!info "Note"
    Vous pouvez recréer tous les graphiques avec des informations mises à jour. Pour cela, vous pouvez consulter le projet sur GitHub à [badaboom](https://github.com/vroger11/badaboom). J'utilise également ce dépôt comme modèle pour mes projets Python. N'hésitez pas à y jeter un œil et à me faire part de vos suggestions pour l'améliorer !

## Résultats

Vu que cette base recense les impacts boules de feux, je me suis d'abord tourné vers les énergies démissions lumineuses et les énergies d'impacts.
Il est à noter que j'ai mis ces énergies à l'échelle logarithmique, car l'échelle des valeurs et tellement varié que j'ai trouvé plus simple et plus lisible de faire ainsi.

--8<--
assets/interactive_dataviz/badaboom/fireballs/energy_hist.html

assets/interactive_dataviz/badaboom/fireballs/impact_energy_hist.html
--8<--

On s'aperçoit que la majorité des boules de feu recensées sont dans le même ordre de grandeur avec quelques exceptions pouvant émettre jusqu'à 375000 GJ d'énergie lumineuse (avec une énergie d'impact de 440kt).
Ces valeurs sont difficiles à appréhender sans référentiels et me laisse rêveur sur l'effet que l'on doit ressentir si on a la chance d'observer en réel un tel phénomène (tout en étant à une distance permettant d'être en sécurité bien sûr 😜).

Avant de vous montrer la carte des impacts enregistrés par le CNEOS, il faut noter que sur leur base de données il manque des localisations pour certaines boules de feu.
C'est pourquoi j'ai réalisé l'histogramme qui suit:

--8<--
assets/interactive_dataviz/badaboom/fireballs/missing_locations.html
--8<--

On peut néanmoins noter qu'il y a de moins en moins de détection réalisée sans localisation.
C'est donc à partir des données possédant une géolocalisation que j'ai réalisé la carte qui suit.
Cette carte est interactive, en passant la souris dessus les bulles vous pouvez voir les détails pour chaque impact.
Vous pouvez également zoomer sur la carte pour voir plus précisément certaines régions du globe.
La taille des bulles correspondant à l'énergie d'impact de la boule de feu.

!!!info "Note"
    La carte ci-dessous a perdu son arrière-plan car l'API précédente n'est plus disponible. J'ai mis à jour l'API dans [badaboom](https://github.com/vroger11/badaboom) en utilisant Plotly avec Mapbox, remplaçant ainsi la solution basée sur Bokeh. Vous pouvez recréer la carte vous-même. Je n'ai pas hébergé le résultat, car l'API devient payante après un certain nombre de vues.

--8<--
assets/interactive_dataviz/badaboom/fireballs/map.html
--8<--

Pour rappel tout le code nécessaire pour réaliser ces figures est disponible sur ma librairie [badaboom](https://github.com/vroger11/badaboom)).

J’espère que cela aidera/inspireront certains d’entre vous.
Dans tous les cas, j'espère que cela a été intéressant.

À bientôt, Vincent.
