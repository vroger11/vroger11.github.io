---
comments: true
title:  "Migration de ce site web vers MkDocs"
date:   2024-09-25
tags:
    - Dévelopement
hide:
    - navigation
excerpt:
---

Après avoir maintenu ce site avec Jekyll pendant quelques années, j'ai décidé qu'il était temps de changer. Autant j'appréciais la flexibilité offerte par Jekyll, les besoins de mes projets avaient évolué. J'utilisais déjà MkDocs pour documenter des projets Python, alors j'ai décidé de tout consolider sous MkDocs. Cet article explique la raison de cette migration, les avantages, les inconvénients, et pourquoi j'ai remplacé Disqus par Giscus pour gérer les commentaires.

## Pourquoi passer de Jekyll à MkDocs ?

Jekyll est un excellent générateur de site statique, surtout si vous hébergez sur GitHub Pages. Il prend bien en charge les blogs, les thèmes, et les plugins, mais il ne répondait plus à tous mes besoins. Il manquait une barre de recherche, un mode clair/sombre, et mon site semblait bricolé dans son ensemble. Maintenir des plateformes séparées pour le blog et la documentation commençait à être contre-productif. MkDocs, un générateur de site statique conçu principalement pour la documentation, est apparu comme l'alternative parfaite pour tout unifier.

## Avantages d'utiliser MkDocs pour mon site

### Documentation intégrée

MkDocs est conçu pour la documentation, ce qui le rend plus facile à utiliser pour créer des tutoriels avec des blocs de notes, une barre de recherche et des fonctionnalités de navigation. Cette intégration me permet de travailler facilement entre mes blogs et mes articles techniques sans changer de framework.

### Workflow simplifié

MkDocs est extrêmement simple. Avec sa structure basée sur Markdown, il n'est pas nécessaire de gérer des langages de template complexes tout comme Jekyll. Je peux tout écrire en Markdown, ce qui accélère considérablement la création de contenu.

### Support multilingue simplifié

L'un de mes besoins essentiels était de prendre en charge à la fois l'anglais et le français. Bien que Jekyll fournisse des moyens de gérer les sites multilingues, MkDocs dispose de plugins comme [mkdocs-static-i18n](https://ultrabug.github.io/mkdocs-static-i18n/) qui simplifient la gestion des traductions des pages. Il est également plus facile de suivre la documentation spécifique à chaque langue sans structures de dossiers ou configurations compliquées.

### Thème Material pour MkDocs

Le thème Material pour MkDocs offre une apparence élégante et moderne dès l'installation. Avec quelques ajustements, j'ai pu donner un aspect visuellement attrayant au blog sans passer beaucoup de temps sur du CSS personnalisé ou sur le développement de thèmes, contrairement à Jekyll où je devais souvent bricoler les thèmes pour obtenir un résultat correct.

## Inconvénients de passer à MkDocs

### Moins de plugins pour les blogs

Jekyll dispose d'un écosystème riche en plugins spécialement conçus pour les blogueurs, tandis que MkDocs en a moins. Par exemple, la gestion d'une série d'articles ou de taxonomies personnalisées est plus mature avec Jekyll, tandis que MkDocs nécessite quelques contournements pour obtenir des fonctionnalités similaires. Néanmoins, je n'utilisais pas cette fonctionnalité, donc ce n'est pas une grosse perte pour moi.

### Flexibilité limitée des thèmes

MkDocs est plus rigide en termes de personnalisation des thèmes par rapport à Jekyll. Bien que le thème Material soit agréable visuellement, il ne permet pas le même niveau de personnalisation que les thèmes Jekyll sans plonger dans le CSS personnalisé.
Cela est un gros avantage pour moi, car j'ai passé trop de temps à modifier le thème par défaut de Jekyll, pour un résultat quelque peu moyen.

## Giscus au lieu de Disqus

Un autre changement technique que j'ai effectué est le passage de Disqus à Giscus pour la gestion des commentaires. Voici pourquoi :

### Respect de la vie privée et performances

Disqus, bien qu'il soit populaire, compromet la vie privée et les performances des pages. Il injecte des publicités et charge plusieurs trackers tiers, ce qui ralentit le chargement des pages. Giscus, en revanche, est léger et utilise GitHub Discussions pour les commentaires. Il est rapide, sans publicité, et correspond mieux à mon approche axée sur la confidentialité.

### Intégration avec GitHub

Comme j'utilise déjà GitHub pour le contrôle de version et la collaboration sur divers projets, Giscus intègre parfaitement les commentaires dans GitHub Discussions. Cela crée un espace unifié où les utilisateurs peuvent non seulement laisser des commentaires, mais aussi participer à des discussions liées aux articles de blog spécifiques.

### Open Source

Giscus est open source, ce qui correspond à ma préférence pour l'utilisation d'outils ouverts et transparents autant que possible. Le caractère propriétaire de Disqus et son modèle basé sur la publicité avaient été un inconvénient pour moi pendant un certain temps, donc Giscus était un choix naturel en cherchant des alternatives.

## Conclusion

Passer de Jekyll à MkDocs a simplifié mon workflow, rendant plus facile la gestion à la fois du site et de mon blog. Bien que MkDocs ne soit peut-être pas le premier choix pour les blogueurs traditionnels, sa simplicité et son orientation vers la documentation en font un outil puissant pour les créateurs de contenu technique comme moi. La transition m'a permis de simplifier mon installation et de tout organiser de manière plus cohérente.

Pour ceux qui gèrent des blogs axés sur la documentation ou qui ont des projets techniques nécessitant une documentation détaillée, je recommande fortement d'essayer MkDocs (surtout avec le thème Material). Et si vous cherchez un système de commentaires rapide et respectueux de la vie privée, Giscus est un choix parfait.

J'espère que cet article aidera ou inspirera certains d'entre vous.
Si vous avez des suggestions, n'hésitez pas à commenter ou à me contacter.

À bientôt, Vincent.
