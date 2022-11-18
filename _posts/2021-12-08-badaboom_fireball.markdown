---
layout: post
comments: true
title:  "Badaboom (part 2) - Fireballs"
ref: fireballs
date:   2021-12-15 8:00:00 +0200
categories: blog dataviz
category: blog
lang: en
---

In this new blog post, I'm going to present you the continuation of my work on my library [badaboom](https://github.com/vroger11/badaboom)).
This article follows my previous article about my badaboom project (you can find this article [here](/blog/dev/2021/09/12/badaboom.html)).
For this second part, I realized the analysis of fireballs.
Fireballs are astronomical terms for meteors that are exceptionally bright and spectacular enough to be seen over a wide area.

For this work, I used the CNEOS database (owned by NASA).
For this article, I will start by presenting you the elements present in this database and finish by showing you the first results I obtained (I will update this post as I progress).

# Results

Since this database lists fireball impacts, I first turned to the light emission energies and the impact energies.
Note that I put these energies on a logarithmic scale, because the scale of values is so varied that I found it simpler and more readable to do so.

{% include badaboom/fireballs/energy_hist.html %}

{% include badaboom/fireballs/impact_energy_hist.html %}

We can see that most of the fireballs recorded are in the same order of magnitude with some exceptions that can emit up to 375000 GJ of light energy (with an impact energy of 440kt).
These values are difficult to apprehend without a reference and let me dream about the effect that we must feel if we have the chance to observe in real such a phenomenon (while being at a distance allowing to be safe of course 😜).

Before showing you the map of the impacts recorded by the CNEOS, it should be noted that on their database there are missing locations for some fireballs.
That's why I made the following histogram:

{% include badaboom/fireballs/missing_locations.html %}

Nevertheless, it can be noted that there are less and less detections made without localization.
It is therefore from the data with a geolocation that I made the following map.
This map is interactive, by passing the mouse over the bubbles you can see the details for each impact.
You can also zoom on the map to see more precisely some regions of the world.
The size of the bubbles corresponds to the impact energy of the fireball.

{% include badaboom/fireballs/map.html %}

As a reminder, all the code needed to make these figures is available on my library [badaboom](https://github.com/vroger11/badaboom)).

I hope this will help/inspire some of you.
In any case, I hope it was interesting.

See you again, Vincent.
