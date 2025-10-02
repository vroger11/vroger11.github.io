---
comments: true
date:   2025-10-02
tags:
    - Automatisation
    - Développement
hide:
    - navigation
excerpt: ""
---

# Constructions Docker efficaces avec `uv`

La résolution de dépendances et l'empaquetage Python sont souvent des points douloureux dans les workflows Dockerisés. Les outils traditionnels comme `pip` et `pip-tools` fonctionnent, mais sont lents et sujets à casser le cache.
Voici [`uv`](https://docs.astral.sh/uv/), un gestionnaire de paquets Python ultra-rapide écrit en Rust par [Astral](https://astral.sh), compatible avec `pip`, `pip-tools`, et les projets suivant PEP 517/518.

Dans les pipelines MLOps modernes, Docker joue un rôle central pour garantir des environnements cohérents entre les expériences locales, les workflows d'intégration continue (CI) et les déploiements en production.
Que ce soit pour empaqueter un service d’inférence ou une image d’entraînement pour des charges à base de GPU, la containerisation garantit la reproductibilité et simplifie la gestion des dépendances.
Associé au CI/CD, cela permet de construire, tester et déployer automatiquement aussi bien les artefacts d’entraînement que de déploiement.
Cependant, la résolution des dépendances Python ralentit souvent la construction des images.

Ce post propose une solution rapide et reproductible avec uv, qui permet de construire des images Python pour l’entraînement ou l’inférence plus vite et plus fiablement.
Ce n’est pas un tutoriel sur uv ni sur Docker. Si vous en avez besoin, commence avec le [guide officiel uv](https://docs.astral.sh/uv/getting-started/) et consulte les [cours IBM sur Coursera](https://www.coursera.org/learn/ibm-containers-docker-kubernetes-openshift) pour Docker.

## ⚡ Pourquoi `uv` ?

`uv` est conçu pour être **rapide, moderne et compatible en remplacement direct** avec les workflows Python existants.
Il est écrit en Rust pour des performances optimales (comparé à `poetry` qui est écrit en Python).
Il installe les dépendances à partir de `pyproject.toml` et `uv.lock`, supprimant le besoin de `pip`, `pip-tools` et `virtualenv`.

Disponible en binaire statique ou via script d’installation, il produit des builds déterministes.
Comparé à `poetry`, `uv` est ultra rapide (plus de 5x plus vite sur mes projets), ce qui m’a poussé à migrer tous mes projets vers `uv`.

## Utiliser une image uv officielle

Astral fournit des images de `uv` officielles pour `python:slim`, `debian:bookworm-slim` et d’autres.
Consultez la documentation officielle pour la liste complète des images Docker :
🔗 [https://docs.astral.sh/uv/guides/integration/docker/#using-the-official-docker-image](https://docs.astral.sh/uv/guides/integration/docker/#using-the-official-docker-image)

Une fois que vous avez trouvé celle qui correspond à vos besoins, vous pouvez l’utiliser ainsi :

```dockerfile
FROM astralsh/uv:latest

# Copier le minimum nécessaire pour résoudre les dépendances
COPY pyproject.toml uv.lock .python-version .
RUN uv sync --no-dev --frozen

# Copier le reste du projet
ADD main.py /workspace

# Point d’entrée par défaut
CMD uv run python main.py
```

On copie `pyproject.toml`, `uv.lock` et `.python-version` avant le reste du projet pour que Docker puisse mettre en cache la couche d’installation des dépendances.
De cette manière, les dépendances ne sont réinstallées que si l’un de ces fichiers change, ce qui rend les reconstructions beaucoup plus rapides et évite une invalidation inutile du cache lorsque vous modifiez votre code source.

## Personnaliser ton image pour utiliser uv

Si vous avez besoin de quelque chose de plus avancé et qu’aucune image Docker officielle d’Astral ne correspond à vos besoins, vous pouvez créer la vôtre.
Voici un setup propre et efficace pour le développement local et les workloads d’inférence, par exemple sur une machine avec GPU :

```dockerfile
FROM nvidia/cuda:12.3.0-runtime-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV UV_COMPILE_BYTECODE=1
SHELL ["/bin/bash", "--login", "-c"]

# Installer uv
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates build-essential
ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="/root/.local/bin/:$PATH"

# Définir le dossier de travail
WORKDIR /workspace
ENV HOME=/workspace

# Copier les fichiers du projet pour les dépendences
COPY pyproject.toml uv.lock .python-version /workspace/

# Installer les dépendances avec uv
RUN uv sync --no-dev --frozen

# Copier le reste du projet
ADD main.py /workspace

# Commande par défaut
CMD uv run python main.py
```

Dans cet exemple :

* `uv` compile en bytecode lors du sync pour accélérer le démarrage (au prix d’une image un peu plus grosse).
* `uv sync --no-dev --frozen` assure des builds reproductibles de niveau production.
* Les dépendances ne sont réinstallées que si `pyproject.toml`, `uv.lock` ou `.python-version` changent.
* Aucun Python système n’est installé. Seul le Python requis par uv dans son venv est utilisé.
* Un dossier `workspace` contient tout le code et l’environnement virtuel de uv (séparation claire).

La commande `uv run` se comporte comme `poetry run` ou `pipenv run` sans surcharge.

Si vous devez installer `uv` dans un autre dossier (par exemple pour des droits d’accès liés à une plateforme web comme OVH), vous pouvez faire ainsi :

```dockerfile
FROM nvidia/cuda:12.3.0-runtime-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV UV_COMPILE_BYTECODE=1
SHELL ["/bin/bash", "--login", "-c"]

# Définir le dossier de travail
WORKDIR /workspace
ENV HOME=/workspace

# Installer uv
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates build-essential
ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="$HOME/.local/bin:$PATH"

# Copier et installer les dépendances
COPY pyproject.toml uv.lock .python-version /workspace/
RUN uv sync --no-dev --frozen

# Copier les fichiers applicatifs
ADD main.py /workspace/

# Entrypoint par défaut
CMD uv run python main.py
```

Là, vous changez le dossier HOME avant d’installer uv (qui est utilisé par l’installateur uv) et vous modifiez le PATH pour qu’il pointe vers le nouvel emplacement d’installation, garantissant ainsi que le binaire uv soit disponible dans tout le conteneur.

## 🧩 Conclusion

Utiliser `uv` dans tes builds Docker apporte :

* Des builds d’images plus rapides que `pip`, `poetry` ou `conda`.
* Une résolution de dépendances déterministe et figée.

Que vous travaillez en local ou que vous déployez dans le cloud, `uv` simplifie et accélère la gestion des environnements Python.

Bonne pratique : ajoute `.venv` dans ton `.dockerignore`. Ça t’évitera de déboguer un mauvais environnement par erreur.
Cela peut arriver en utilisant par mégarde `COPY . /workspace`, à éviter aussi.

---

Merci d’avoir lu, j’espère que ce post t’a été utile et inspirant.

Une partie de ce post est inspirée du [guide officiel d’intégration Docker](https://docs.astral.sh/uv/guides/integration/docker/) et de mon expérience.

Si vous avez des remarques ou suggestions, n’hésite pas à partager tes idées ou conseils.
