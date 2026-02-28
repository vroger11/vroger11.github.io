---
comments: true
title:  "L'expérience de mon doctorat"
date:   2022-10-30
tags:
    - Archives
    - Thèse
hide:
  - navigation
excerpt: Petite rétrospective sur ma thèse et résumé de mon travail.
---

Cela fait un moment, pas de bilan de deuxième année en raison de sentiments mitigés pendant le covid. Mais grâce à mes encadrants, j'ai réussi à surmonter mes pensées négatives. Je suis maintenant fier d'être un docteur en informatique 🎉.

Pour rappel, cette thèse a été encadré par [Julien Pinquier](https://www.irit.fr/~Julien.Pinquier/), [Jérôme Farinas](https://www.irit.fr/~Jerome.Farinas/) et [Virginie Woisard](https://octogone.univ-tlse2.fr/accueil/membres/virginie-woisard--183287.kjsp). Elle a été réalisé à l'[IRIT](https://www.irit.fr/) (Toulouse, France).

Dans ce post, je vais partager avec vous un résumé de mon travail, quelques sentiments et conseils que j'ai à partager.

## Résumé de ma thèse

Les personnes atteintes de cancers des voies aérodigestives supérieures présentent des difficultés de prononciation après des chirurgies ou des radiothérapies. Il est important pour le praticien de pouvoir disposer d’une mesure reflétant la sévérité de la parole.
Pour produire cette mesure, il est communément pratiqué une étude perceptive qui rassemble un groupe de cinq à six experts cliniques. Ce procédé limite l'usage de cette évaluation en pratique.
Ainsi, la création d'une mesure automatique, semblable à l'indice de sévérité, permettrait un meilleur suivi des patients en facilitant son obtention.

Pour réaliser une telle mesure, nous nous sommes appuyés sur une tâche de lecture, classiquement réalisée. Nous avons utilisé les enregistrements du corpus cancer qui rassemble plus de 100 personnes [^1].
Ce corpus représente environ une heure d'enregistrement pour modéliser l'indice de sévérité.

Dans ce travail de doctorat, une revue des méthodes de l'état de l'art sur la reconnaissance de la parole, des émotions et du locuteur utilisant peu de données a été entreprise.
Nous avons ensuite essayé de modéliser la sévérité à l'aide d'apprentissage par transfert et par apprentissage profond.
Les résultats étant non utilisables, nous nous sommes tourné sur les techniques dites <<~few shot~>> (apprentissage à partir de quelques exemples seulement).
Ainsi, après de premiers essais prometteurs sur la reconnaissance de phonèmes [^2], nous avons obtenu des résultats prometteurs pour catégoriser la sévérité des patients.
Néanmoins, l'exploitation de ces résultats pour une application médicale demanderait des améliorations.

Nous avons donc réalisé des projections des données de notre corpus. Comme certaines tranches de scores étaient séparables à l'aide de paramètres acoustiques, nous avons proposé une nouvelle méthode [^3]. Celle-ci est fondée sur des représentations de la parole autoapprise sur le corpus Librispeech : le modèle PASE+, qui est inspiré de l’Inception Score (généralement utilisé en image pour évaluer la qualité des images générées par les modèles).
Notre méthode nous permet de produire un score semblable à l'indice de sévérité avec une corrélation de Spearman de 0,87 sur la tâche de lecture du corpus cancer.
L'avantage de notre approche est qu'elle ne nécessite pas des données du corpus cancer pour l'apprentissage.
Ainsi, nous pouvons utiliser l'entièreté du corpus pour l'évaluation de notre système.
La qualité de nos résultats nous a permis d’envisager une utilisation en milieu clinique à travers une application sur tablette : des tests sont d'ailleurs en cours à l'hôpital Larrey de Toulouse.

[^1]: I participated in the analysis of the dataset in [hal-02921918](https://hal.archives-ouvertes.fr/hal-02921918)

[^2]: I conducted the review of state-of-the-art techniques and the experiments that lead to this paper:  [DOI:10.1186/s13636-022-00251-w](http://dx.doi.org/10.1186/s13636-022-00251-w)

[^3]: This work led to the following publication (yet to be published): Roger, V., Farinas, J., Woisard, V., and Pinquier, J. (2022b). Création d’une mesure entropique de la parole pour évaluer l’intelligibilité de patients atteints de cancers des voies aérodigestives supérieures. In 34e Journées d’Études sur la Parole (JEP2022).

**Mots clés:** Parole pathologique, indice de sévérité, trouble de la parole, cancer ORL, apprentissage profond, apprentissage avec quelques exemples, auto supervisé, mesure entropique, few-shot, peu de données, quantité de données limités, traitement automatique de la parole.

Pour mon manuscrit, j'ai utilisé Latex (comme tous mes articles). Pour collaborer avec d'autres personnes (notamment mes encadrants) sur celui-ci, j'ai utilisé overleaf pour avoir des commentaires collaboratif (plus d'informations [ici](https://docs.overleaf.com/collaborating/commenting)), un historique des modifications et pour synchroniser mon travail en cours sur GitHub (par sécurité car je ne voulais pas réécrire ce manuscrit depuis le début).
Une fois mon manuscrit mis en ligne, j'ajouterai un lien vers ce dernier ici pour les personnes intéressées.

## La présentation de ma thèse

Comme pour mon manuscrit, j'ai utilisé Latex avec beamer pour ma présentation! Ce n'est pas un choix courant (je n'ai pas vu de présentation de thèse utilisant Latex) mais si on est habitué, on gagne tellement de temps pour se concentrer sur le fond plutôt que sur la forme. J'ai créé mon propre thème (pas à partir de zéro je ne suis pas un fou 🤣) et il était presque terminé avant de travailler sur ma présentation de thèse (car j'utilisais beamer pour mes présentations hebdomadaires avec mes encadrants).

Maintenant parlons un peu de mon ressenti, j'étais stressé c'est sûr, ce moment m'intimidait... C'est tellement formel, surtout le temps pour réaliser la présentation (45 min dans mon cas). D'habitude quand je présente des choses, si je prends 5/10 de minutes de plus ou de moins que prévu, c'est OK. Mais là, je n'avais pas cette marge d'erreur. Cela m'a stressé et même si j'ai beaucoup répété pour cette présentation, plus je me préparais, plus je me sentais stressé. Ce qui est amusant, c'est que lorsqu'il y avait des problèmes (des gens qui arrivaient alors que j'avais commencé, des diapositives manquantes, des diapositives qui n'étaient pas partagées au début et ainsi de suite), cela me faisait oublier les aspects stressants de l'expérience.
Pour ceux qui n'étaient pas là, vous pouvez voir ma présentation/"performance":

<iframe width="720" height="480"
    src="https://www.youtube.com/embed/yvYZDBKdzB8">
</iframe>

Je pense que j'aurais pu réduire mon stress si j'avais fait une répétition dans l'amphithéâtre et utilisé un chronomètre numérique (avec un grand écran) lors de ma répétition et le jour de la soutenance.

J'étais plus à l'aise avec les questions, là je n'étais pas limité par le temps pour répondre au jury. De plus, le fait que ce soit interactif réduit l'aspect formel de l'épreuve. Puis, après la délibération du jury, j'ai reçu mon titre de docteur ! Quel sentiment incroyable, un mélange de joie, de stress et de soulagement. Tout cela m'a fait pleurer à la fin, quel grand moment !

J'espère que cela aidera et/ou inspirera certains d'entre vous.

À bientôt, Vincent.
