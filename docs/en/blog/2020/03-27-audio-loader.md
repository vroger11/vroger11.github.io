---
comments: true
title:  "Audio loader"
date:   2020-03-27
tags:
    - Archive
    - Development
    - AI
hide:
    - navigation
excerpt: Library developed in Python and oriented to be used with Pytorch.
---

!!!info "Note"
    This project began as a side endeavor alongside my thesis. However, with the emergence of `torchaudio` and `speechbrain`, I no longer saw the need to continue it.

Today, I released an early version of the library I am working on:
It is called [Audio Loader](https://github.com/vroger11/audio_loader) and it loads audio batches (with features and ground truth) for deep learning libraries such as PyTorch or TensorFlow.

For now, it is only an early work and have basic mechanisms implemented such as:

- PyTorch bindings (basic one that fill all RAM and designed for windowed sampler).
- Ground truth generic loader, with TIMIT example and C2SI example.
- Generic samplers (loading using windows or use the full files).
- Basic features using [librosa](https://librosa.github.io/librosa/) framework (only raw audio and MFCC for now).
- Basic activity detection system to filter features.
- Most features have a corresponding testing module using [pytest](https://docs.pytest.org).

My TODO list for the future (it is not an exhaustive one):

1. Adding a documentation website.
2. Add other features (spectrograms and so on)
3. Have an elegant system of cache as an alternative to fill everything into the RAM.
4. TensorFlow bindings

For more details look into the [GitHub repository](https://github.com/vroger11/audio_loader).
If you want to contribute, feel free to contact me or propose your pull request 😄.

Hope it helps some of you.

Cheers, Vincent
