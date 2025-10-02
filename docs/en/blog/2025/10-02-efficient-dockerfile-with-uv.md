---
comments: true
date:   2025-10-02
tags:
    - Automation
    - Development
hide:
    - navigation
excerpt: ""
---

# Efficient Docker Builds with `uv`

Python packaging and dependency resolution can be long pain points in Dockerized workflows. Traditional tools like `pip` and `pip-tools` are functional, but slow and prone to cache-busting.
Enter [`uv`](https://docs.astral.sh/uv/), a blazing-fast, Rust-based Python package manager built by [Astral](https://astral.sh), compatible with `pip`, `pip-tools`, and PEP 517/518 projects.

In modern MLOps pipelines, Docker plays a central role in ensuring consistent environments across local experiments, CI workflows, and production deployments.
Whether you're packaging an inference service or a training image for GPU-based workloads, containerization guarantees reproducibility and simplifies dependency management.
When combined with CI/CD, this enables automated building, testing, and deployment of both training and serving artifacts.
However, Python dependency resolution often slows down image builds.

This post provides a fast and reproducible solution using uv, allowing you to build Python images for training or inference faster and more reliably.
This is not a tutorial on uv nor docker. If you need one, you can start with the official [uv getting started](https://docs.astral.sh/uv/getting-started/) by the uv team and look at the [IBM courses on coursera](https://www.coursera.org/learn/ibm-containers-docker-kubernetes-openshift) for docker.

## ⚡ Why `uv`?

`uv` is designed to be **fast, modern, and drop-in compatible** with existing Python workflows.
It is written in Rust for performance (compared to `poetry` which is written in Python).
It installs dependencies from `pyproject.toml` and `uv.lock` which replaces the need of `pip`, `pip-tools`, and `virtualenv`.

It is available as a static binary or via install script and produces deterministic builds.
Compared to `poetry`, `uv` it is ultra fast (more than 5x faster on my projects) and this made me migrate all my projects to `uv`.

## Using an official uv image

Astral provides official `uv` base images for `python:slim`, `debian:bookworm-slim` and others.
See the official docs here for the list of official docker images:
🔗 [https://docs.astral.sh/uv/guides/integration/docker/#using-the-official-docker-image](https://docs.astral.sh/uv/guides/integration/docker/#using-the-official-docker-image)

Once you found the one fitting your needs, you can use it as follows:

```dockerfile
FROM astralsh/uv:latest

# Copy the minimum to compute dependencies
COPY pyproject.toml uv.lock .python-version .
RUN uv sync --no-dev --frozen

# Copy rest of the project
ADD main.py /workspace

# Default entry point
CMD uv run python main.py
```

We copy `pyproject.toml`, `uv.lock`, and `.python-version` before the rest of the project so Docker can cache the dependency installation layer.
This way, dependencies are only reinstalled if one of those files changes, which makes rebuilds much faster and avoids unnecessary cache invalidation when editing your source code.

## Customizing your image to use uv

If you need something fancier and no official docker images from astral fit your needs, you can create your own.
Let’s start with a clean and efficient setup for local development and inference workloads, like on a GPU-enabled machine:

```dockerfile
FROM nvidia/cuda:12.3.0-runtime-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV UV_COMPILE_BYTECODE=1
SHELL ["/bin/bash", "--login", "-c"]

# Install uv
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates build-essential
ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="/root/.local/bin/:$PATH"

# Set working directory
WORKDIR /workspace
ENV HOME=/workspace

# Copy project files for dependencies
COPY pyproject.toml uv.lock .python-version /workspace/

# Install dependencies using uv
RUN uv sync --no-dev --frozen

# Copy rest of the project
ADD main.py /workspace

# Default command
CMD uv run python main.py
```

In this example we:

* Forced `uv` to compile into bytecode when syncing which speedup startup at the cost of a slightly bigger image.
* Used `uv sync --no-dev --frozen` for reproducible, production-grade builds.
* Avoids reinstalling dependencies unless `pyproject.toml` or `uv.lock` and `.python-version` changes.
* Avoided installing any system-wide Python. Only the required Python in our uv venv will be installed.
* Used a workspace folder for all copied codes and the venv created for uv (to keep things well separated).

The `uv run` command behaves like `poetry run` or `pipenv run` without overhead.

If you need to install uv in another folder (for access right linked to a web platform like OVH) you can do as follows:

```dockerfile
FROM nvidia/cuda:12.3.0-runtime-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV UV_COMPILE_BYTECODE=1
SHELL ["/bin/bash", "--login", "-c"]

# Set working directory
WORKDIR /workspace
ENV HOME=/workspace

# Install uv
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates build-essential
ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="$HOME/.local/bin:$PATH"

# Copy and install dependencies
COPY pyproject.toml uv.lock .python-version /workspace/
RUN uv sync --no-dev --frozen

# Copy application files
ADD main.py /workspace/

# Default entrypoint
CMD uv run python main.py
```

There you change the HOME folder before installing `uv` (which is used by the `uv` installer) and you change the PATH to point to the new installation location, ensuring that the uv binary is available system-wide inside your container.

## 🧩 Final Thoughts

Using `uv` in your Docker builds provides:

* Faster image builds compared to `pip`, `poetry` or `conda` based images.
* Deterministic and frozen dependency resolution.

Whether you're working locally or deploying to a cloud platform, `uv` streamlines Python environments with simplicity and speed.

As a best practice add `.venv` to your `.dockerignore` file it will save you time in debugging the wrong environment.
It can happen by mistake  using `COPY . /workspace` which you should also avoid.

---

Thanks for reading, I hope it was insightful and inspiring.

Part of this post is inspired from the [official Docker instruction guide](https://docs.astral.sh/uv/guides/integration/docker/) and my experience.

If you have any remarks or suggestions feel free to share your ideas/advice.
