---
layout: post
comments: true
title:  "Revue de ma première année de thèse"
ref: first_year_review
date:   2019-10-12 17:15:00 +0200
categories: blogue these
lang: fr
---

Après presque une année (11 mois), je vais partager dans cet article mon expérience en tant qu'étudiant en thèse.
Dans cet article de blogue, je vais parler du sujet de ma thèse, la direction prise, des travaux futurs et des changements dans ma vie (liés à la thèse).

Avant de continuer, je voudrais remercier mes encadrants[^1] pour leur aide et leurs conseils, [La Région Occitanie](https://www.laregion.fr/) et [l'Université Fédérale de Toulouse](https://en.univ-toulouse.fr) pour leur financement, l'Université Paul Sabatier et le Laboratoire IRIT pour leur support.
Je souhaite également rappeler que c'est un article de blogue personnel (il n'engage que moi-même).

# Sujet
Ma thèse consiste à créer un Système Automatique de Mesure de l'Intelligibilité (SAMI).
En traitement de la parole, l'intelligibilité peut correspondre à plusieurs choses.
Avant de définir ce terme, regardons un petit schéma général correspondant au système d'écoute chez l'humain:

![image](/assets/images/listener_understanding_fr.png)

Dans la communauté de la parole, l'intelligibilité désigne généralement que le signal écouté est 100% clair.
Cette mesure sert à identifier les perturbations liées aux bruits environnementaux.
Comme vous l'avez surement deviné, ce n'est absolument pas la mesure que je cherche durant cette thèse.

Pour cette thèse, nous nous intéressons à la clarté de l'orateur indépendamment des bruits environnementaux et de toutes connaissances de l'auditeur.

Ainsi, par intelligibilité je parle de celle de l'orateur.

Ce genre de mesure a plusieurs applications:
* l'évaluation de la prononciation de personnes apprenant des langues étrangères.
* le raffinement des informations pour les dispositifs auditifs afin aider à leur amélioration.
* l'évaluation des dommages d'une ou plusieurs maladie(s) (tel que Parkinson ou les cancers buccaux) pour aider au processus de rééducation.

Le dernier point correspond au focus de ma thèse.
Par conséquent, mon système doit fonctionner sur de la parole pathologique et non pathologique.

Pour atteindre cet objectif, j'ai accès au [corpus C2SI](https://www.researchgate.net/publication/333132284_Construction_of_an_automatic_Carcinologic_Speech_Severity_Index_C2SI_score).
Il contient environ 2 heures d'enregistrements de plusieurs tâches (lecture, parole spontanée, A maintenue...).
Tous les sujets sont des Français et parlent couramment le Français.
Le corpus C2SI est composé d'enregistrements de patients (souffrant de cancers buccaux) et d'un groupe contrôle (participants n'ayant pas de trouble de la parole)
Plus important encore, dans ce corpus, nous avons accès à une évaluation subjective de l'intelligibilité de chaque participant.
Ces scores ont été réalisés par un jury de 5 experts nous donnant un score compris entre 0 et 10.
Ce corpus est donc le point de départ de ma thèse.

# Direction prise pour la thèse

Je suis un scientifique de la donnée et j'aime particulièrement laisser la machine apprendre les concepts pour moi.
C'est pour ça que les encadrants et moi-même avons décidé de faire apprendre le concept d'intelligibilité à l'aide de technique d'apprentissage par machine avec des systèmes de bout en bout (basés sur de l'apprentissage profond).

Après quelques expériences sur le corpus C2SI, j'en tire les leçons suivantes:

* Utiliser les techniques d'apprentissage de l'état de l'art en reconnaissance de la parole (architectures très profondes et larges) est impossible sur la quantité de données que j'ai à disposition

* Utiliser des modèles avec moins de paramètres nécessaires (avec des techniques comme [SincNet](https://arxiv.org/abs/1808.00158) et l'utilisation de petites architectures) ne solutionne pas suffisamment bien le problème sur de la parole pathologique.

* L'apprentissage par transfert (utilisant des modèles appris sur de la parole considérée non pathologique) résulte en des performances plus faibles que les modèles avec moins de paramètres.

Pour le moment, je ne peux obtenir plus de données (parce que cela peut être douloureux pour les patients) pour obtenir de meilleurs résultats.
L'augmentation de données n'a pas aidé à obtenir des résultats suffisants.
Après avoir écouté l'enregistrement de plusieurs participants, je suis maintenant capable de deviner l'intervalle de valeur de leur score obtenu.
Cela m'a rappelé certaines de mes lectures parlant de techniques qui essayaient d'être aussi efficientes que les enfants pour apprendre de nouveaux concepts (en termes de quantité de données): les techniques dites de few-shot.
Comme cela correspond à mon problème, j'ai commencé à faire une revue de ce domaine.
Pour partager cette expertise acquise, j'ai commencé une revue de littérature sur les techniques de few-shot pour le traitement de la parole.

# À venir

Mes publications prévues (et futurs articles de blogue) sont les suivants:

1. Une revue de littérature sur les techniques de few-shot appliquées pour le traitement de la parole.

2. Des expériences utilisant les techniques de few-shot sur le corpus du C2SI.

# Ressenti et impact sur mon mode de vie

J'ai arrêté le dessin digital parce que cela me nécessitait des sessions de 2 h.
J'apprends tout depuis le début dans ce domaine (logiciels, techniques de dessin...).
C'est un exercice dur pour moi et qui me prend beaucoup de ressources mentales.
À la place, j'ai commencé mon site internet et ce blogue vu que j'aime partager.
De plus, cela me semble plus simple et plus amusant que je ne l'avais anticipé (c'est surtout dû à la simplicité de [Jekyll](https://jekyllrb.com/))
Je reprendrai surement le dessin digital, mais après ma thèse.

Actuellement, j'apprends le rock acrobatique avec mon amour (ma Krikri).
Je pense qu'avoir trop d'activité physique est néfaste pour le corps, mais aussi pour le moral.
Par conséquent, j'ai arrêté de courir.

En tant qu'informaticien, je suis souvent derrière un bureau.
Ainsi, je suis peu exposé au soleil et produis ainsi moins de vitamine D que les personnes qui travaillent dehors.
Pour compenser, je prends de la vitamine D3.
Je vous conseille de regarder [ici](https://www.julienvenesson.fr/calculer-son-besoin-en-vitamine-d-en-fonction-de-son-poids/) ou [là](https://www.julienvenesson.fr/eviter-les-fortes-doses-de-vitamine-d/) pour comprendre ma démarche.
Dans tous les cas, demandez conseil à un professionnel.
Une autre solution consiste à s'exposer plus au soleil (tout en profitant pour se relaxer/méditer).
Ainsi, j'évite un maximum le métro en utilisant ma trottinette (une non électrique :smile: ).

Lorsque je ressens moins de motivation que d'habitude, j'écoute les sons suivants (cela pourrait vous remonter le moral):
* [Into the Abyss, by Hilltop Hoods](https://youtu.be/FEvlOHR_624).
* [Here, by Briggs](https://youtu.be/tA07dpATOcY).
* [Fight back, by  Neffex](https://youtu.be/CYDP_8UTAus).

J’espère que cela aidera certains d’entre vous.

À la revoyure, Vincent.


[^1]: [Julien Pinquier](https://www.irit.fr/~Julien.Pinquier), [Jérôme Farinas](https://www.irit.fr/~Jerome.Farinas) et [Virginie Woisard](https://octogone.univ-tlse2.fr/accueil/membres/virginie-woisard--183287.kjsp)
