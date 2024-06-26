---
layout: post
comments: true
title:  "Review of my first year as a PhD student"
ref: first_year_review
date:   2019-10-12 17:15:00 +0200
categories: blog PhD
category: blog
lang: en
excerpt: Talking about the subject of my thesis, the directions we are going for and some feelings and the impact on my life.
---

After almost a year (11 months), here I share with you my first year as a PhD student.
In this post I will talk about the subject of my PhD thesis, the direction it takes, future works and the changes in my life (due to PhD).

Before I continue, I would like to thank my advisors[^1] for their help and advises, [La Région Occitanie](https://www.laregion.fr/) and [Federal University of Toulouse](https://en.univ-toulouse.fr) for funding me, the University Paul Sabatier and the IRIT lab for their support.
Also, I want to remind you that it is my blog (it only engages myself).

## Subject

My thesis consists in creating an Automatic System of Intelligibility Measurement (SAMI in French).
In the speech processing field, intelligibility can mean multiple things.
First, lets define it by looking how a listener listens:

![image](/assets/images/listener_understanding.jpeg)

Classic intelligibility measurements suppose that the signal from the listened speaker is 100% clear and is alterable by the environment.
Hence, they measure the perturbations done by the environment.
It is not what I am focused on for this thesis.

In this thesis, we are interested in the clarity of the speaker independently of any environment and any knowledge from the listener.
Hence, by intelligibility I mean the speaker intelligibility.

This measures have multiple applications:

- evaluate people diction, especially foreigners to help them.
- refine information for hearing aids to improve their efficiency.
- evaluate the damages of one or multiple diseases (such as Parkinson or oral cancers) to help the reeducation process.

The last point is the focus of my thesis.
Hence, my automatic system has to work with pathological speech and unimpaired speech.

To achieve my goal, I have access to the [C2SI corpus](https://www.researchgate.net/publication/333132284_Construction_of_an_automatic_Carcinologic_Speech_Severity_Index_C2SI_score).
It contains around 2 hours of recording for multiple tasks (such as reading, spontaneous speech or maintained A).
All speakers are French and are speaking French.
The dataset contains patients (that suffer from oral cancer) and controls (which have no problems to speak).
More importantly, in this dataset we have access to a subjective evaluation of the intelligibility for every speaker.
A panel of 5 experts produced intelligibility scores valuated between 0 and 10.
This dataset is the starting point of my thesis.

## Thesis direction

I am a data scientist and I like to let the machine learn concepts for me.
It is why my advisors and I decided to learn the concept of intelligibility using machine learning techniques with end-to-end systems (based on deep learning techniques).

After some experiments on the C2SI dataset, it appears to me that:

- Using state-of-the-art learning techniques for speech processing (with big architectures) is impossible on the available amount of data I have.
- Using models with fewer parameters (with techniques such as [SincNet](https://arxiv.org/abs/1808.00158) and use small architectures) does not perform well enough on pathological speech.
- Use pre-trained models on unimpaired speech to do transfer learning performs worse than "light" models.

I can't have more data (as it can be painful for the patients) to obtain better results.
Classic data augmentation isn't a solution either.
After listening some recordings, I can guess the intelligibility range of most speakers.
It reminds me some reading about techniques that try to be as efficient as a baby to learn new concepts (in terms of quantity of data required): few-shot learning.
As it corresponds to my problem, I started to review what existed in that domain.
To share that expertise acquired, I started to write a survey on few-shot learning for speech processing.

## Coming next

My planned publications (and future posts) are the following:

1. Survey on few-shot learning techniques for speech processing.

2. Experiments using promising few-shot techniques on the C2SI dataset.

## Feeling and impact on my way of life

I stopped digital drawing as it took me a least two hours per session.
I learn everything from scratch in that domain (softwares, drawing techniques and so on).
It feels hard and it takes me a lot of mental energy.
Instead, I started my website and this blog as I like to share stuff.
Also, it feels easier and more fun to me as I expected (it is mainly due to the simplicity of [Jekyll](https://jekyllrb.com/)).
Maybe I will redo digital drawing, but after my thesis.

I'm starting learning acrobatic rock with my love (my Krikri).
I believe having too much physical exercises can harm my body but also my moral.
So I stopped running.

As a computer scientist guy, I am often in a building behind a desk.
The sun does not directly go on my skin and I produce less Vitamin D than peoples who work outside.
To compensate I take some vitamin D3, look at [here](https://www.hsph.harvard.edu/nutritionsource/vitamin-d/) or [there](https://www.health.harvard.edu/staying-healthy/time-for-more-vitamin-d) to better understand why I do so.
I recommend you to talk about it with a professional.
Also, to be more exposed to the sun (and meditate/relax) I avoid the subway as much as I can by using my scooter (a manual one 😄).

When I have less motivation than usual, I listen to the following songs (maybe it can cheer you up):

- [Into the Abyss, by Hilltop Hoods](https://youtu.be/FEvlOHR_624).
- [Here, by Briggs](https://youtu.be/tA07dpATOcY).
- [Fight back, by  Neffex](https://youtu.be/CYDP_8UTAus).

Hope it was instructive and can help some of you.

Cheers, Vincent.

[^1]: [Julien Pinquier](https://www.irit.fr/~Julien.Pinquier/index_en.php), [Jérôme Farinas](https://www.irit.fr/~Jerome.Farinas) and [Virginie Woisard](https://octogone.univ-tlse2.fr/accueil/membres/virginie-woisard--183287.kjsp)
