---
comments: true
title: "Benchmark : Pandas, Polars, DuckDB avec des colonnes catégorielles Parquet"
date: 2025-06-22
tags:
  - Données
  - Développement
  - Python
hide:
  - navigation
excerpt: "Benchmark des performances de chargement et d'analyse sur des colonnes catégorielles Parquet : Pandas vs Polars vs DuckDB"
---

Stocker des données de capteurs structurées au format Parquet permet de réduire significativement l’espace disque et l’usage mémoire, en particulier lorsque l’on encode les colonnes catégorielles plutôt que d’utiliser des chaînes de caractères brutes.
Je voulais évaluer comment différentes bibliothèques Python se comparent en termes de **vitesse de chargement**, **performance analytique** et **utilisation mémoire** avec ce type de colonnes.

D’après mon expérience, Polars est souvent le plus rapide sur des données numériques ou de simples chaînes, mais le comportement change lorsqu’on utilise de véritables types catégoriels.

Dans ce billet, je compare trois outils populaires :

- [Pandas](https://pandas.pydata.org)
- [Polars](https://pola.rs)
- [DuckDB](https://duckdb.org)

## 🎯 Motivation

Parquet est un format de stockage en colonnes largement adopté, idéal pour des structures tabulaires comme celles des DataFrames (Pandas/Polars) ou des bases de données relationnelles.
Ce format est particulièrement efficace quand on encode les colonnes catégorielles.

Cependant, les performances varient fortement selon :

- La manière dont les colonnes catégorielles sont gérées
- L’usage d’une exécution paresseuse (lazy) ou immédiate (eager)
- Le coût des conversions internes pendant le chargement ou le calcul

Pour explorer ces différences, j’ai conçu un benchmark basé sur des journaux réels de capteurs portés, où des métadonnées catégorielles (activité, type de capteur, orientation) complètent les valeurs numériques.

Le script de génération du jeu de données est disponible dans la section *Code et reproductibilité*.

### Qu’est-ce qu’une colonne catégorielle et pourquoi les utiliser ?

Les colonnes catégorielles prennent un nombre fini de valeurs distinctes.
Elles sont souvent représentées sous forme de chaînes dans les jeux de données bruts (par exemple `"walking"`, `"running"`), mais peuvent être encodées en entiers pour plus d’efficacité.

Dans Parquet, cet encodage se fait via un dictionnaire : les valeurs sont stockées comme des entiers avec une table de correspondance pour retrouver la chaîne d’origine.
Cela permet un stockage compact tout en conservant la lisibilité lors des requêtes.

Chaque framework gère ces colonnes différemment, mais le format Parquet garantit une compatibilité entre bibliothèques comme Pandas, Polars et DuckDB.

## 📦 Jeu de données : REALWORLD2016

Ce benchmark utilise le [jeu de données REALWORLD2016](https://www.uni-mannheim.de/dws/research/projects/activity-recognition/dataset/dataset-realworld/) composé de mesures accéléromètre, gyroscope et magnétomètre sur 15 individus réalisant différentes activités.

Chaque enregistrement contient :

- Accélérations `x`, `y`, `z`
- Index temporel `time`
- ID utilisateur `user`
- Métadonnées catégorielles :
  - `activity` (ex : marche, saut)
  - `orientation` (ex : torse, cuisse)
  - `sensor` (acc, gyr, mag)

J’ai converti les fichiers CSV compressés d’origine en fichiers Parquet columnaires à l’aide de Pandas et PyArrow. Les colonnes catégorielles ont été typées explicitement au format standard d’Arrow, plus interopérable que les types enum spécifiques à Polars ou DuckDB.
Ainsi, les fichiers générés sont compatibles avec Pandas, Polars et DuckDB.

## 🧪 Tâches de benchmark

Chaque outil a été évalué sur quatre tâches :

1. **Chargement** des fichiers Parquet
2. **Statistiques descriptives** : min, max, moyenne sur les colonnes numériques
3. **Comptes groupés** sur les colonnes `activity`, `orientation` et `sensor`
4. **Utilisation mémoire** : RAM minimale nécessaire pour charger entièrement les données

## 📁 Code et reproductibilité

Tous les scripts et notebooks sont disponibles ici :

👉 [Dépôt GitHub](https://github.com/vroger11/parquet-categorical-benchmark)

Pour reproduire les benchmarks :

```bash
uv sync
uv run python download_extract_realworld.py   # génère les fichiers Parquet
jupyter notebook load_data_benchmarks.ipynb   # exécute les benchmarks
```

Je recommande d’ouvrir le notebook pour consulter les implémentations ainsi que les remarques et astuces ajoutées dans les cellules.
Vos suggestions et contributions sont les bienvenues (issues, pull requests ou commentaires).

## 📈 Résumé des résultats

### Temps de chargement

```mermaid
xychart-beta
    title "Temps de chargement (en secondes)"
    x-axis ["Polars Eager initial", "Polars Eager", "Polars Lazy", Pandas, DuckDB, "df depuis DuckDB"]
    bar [62.0, 9.0, 4.6, 4.5, 3.3, 16.1]
```

La première implémentation en eager de Polars, utilisant la concaténation implicite, est de loin la pire (62 s).
La concaténation explicite avant de passer les données à Polars s’est révélée bien plus performante.
C’est un cas rare où déléguer à la logique interne d’une bibliothèque nuit à la performance.

Polars en lazy est aussi rapide que Pandas, tandis que DuckDB est légèrement plus rapide encore.
Récupérer le DataFrame Pandas depuis DuckDB était sensiblement lent et devrait être évité, car c’est plus lent que de charger les données directement avec Pandas.

!!! note "Note"
    En général, il faut éviter d'appeler `.collect()` sur un DataFrame lazy Polars sans chaîne d’opérations.
    Ici, `.collect()` est volontairement appelé pour isoler et comparer les performances entre outils.
    Même remarque pour DuckDB où il faut passer `hive_partitioning = true` dans votre `read_parquet` afin d’éviter de précharger des données non nécessaires.

### Describe + value_counts

On compare maintenant des opérations analytiques, à commencer par `describe()` :

```mermaid
xychart-beta
    title "Calcul de describe (en secondes)"
    x-axis ["Polars", "Pandas", "DuckDB"]
    bar [1.6, 4.5, 9.8]
```

Les données étant déjà en mémoire, Polars ne peut pas exploiter ses optimisations lazy.
Il reste tout de même le plus rapide.

Puis les comptes groupés (value_counts) :

```mermaid
xychart-beta
    title "Comptes groupés (en secondes)"
    x-axis ["Pandas", "Polars Lazy", "Polars Eager", "DuckDB"]
    bar [1.0, 1.2, 1.5, 0.3]
```

Surprise : Pandas surpasse ici Polars Lazy (1.0 s contre 1.2 s).
DuckDB est bien plus rapide.

!!! warning "Correction"
    Dans une version précédente de ce post, j'avais indiqué une valeur incorrecte pour DuckDB.
    Merci à [Éric Mauvière](https://www.linkedin.com/in/ericmauviere/) d'avoir signalé cette incohérence.

### Utilisation mémoire

On mesure ici la RAM nécessaire pour contenir les données en mémoire.
Attention, certaines bibliothèques utilisent temporairement plus de mémoire pendant le chargement.

```mermaid
xychart-beta
    title "Mémoire nécessaire (en Go)"
    x-axis ["Pandas", "Polars", "DuckDB", "Dataframe depuis DuckDB"]
    bar [2.17, 2.71, 6.33, 11.83]
```

Polars utilise environ 25% de RAM en plus que Pandas, en raison d’Arrow et de son cache interne.

DuckDB consomme presque 3 fois plus que Pandas, car il stocke les colonnes catégorielles comme des chaînes complètes (VARCHAR).

Exporter le DataFrame depuis DuckDB aggrave la situation : on atteint 18.16 Go nécessaires.

## 📋 Conclusion

Chaque outil a ses points forts :

| Outil  | Avantages                        | Inconvénients                   |
| ------ | -------------------------------- | ------------------------------- |
| Pandas | API simple, chargement rapide    | Pas d’exécution lazy            |
| Polars | Analyse très rapide (en lazy)    | Moins performant si mal utilisé |
| DuckDB | Chargement compétitif, SQL natif | Grosse consommation mémoire     |

Polars est très efficace si bien utilisé, mais nécessite de connaître le lazy execution model.
DuckDB reste un bon outil SQL, mais à éviter pour les colonnes catégorielles Arrow, tant qu’elles sont représentées en VARCHAR.

---

*Merci pour la lecture. Vous pouvez ouvrir une issue, proposer une amélioration dans le dépôt, ou laisser un commentaire sur ce billet.*
