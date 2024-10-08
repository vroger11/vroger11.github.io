---
title:  "BadaBoom - Asteroids"
date:   2021-09-12
tags:
    - Archive
    - Visualizations
hide:
    - navigation
excerpt: My first visualizations using asteroid data provided by NASA ('Asteroids - NeoWs' database).
---

In this new blog post, I'm going to present you a little project I've been doing lately.
Being a space enthusiast, I was looking for a simple database that would allow me to better understand the immensity of the world around us.
I finally found the 'Asteroids - NeoWs' database provided by NASA (for more information, look [here](https://api.nasa.gov/)).
This database is focused on asteroids passing near the planet Earth.
In this database, the closest point of an asteroid passing near the Earth represents an event.

So I made a repository named [badaboom](https://github.com/vroger11/badaboom). This repository contains a parser (data extractor) and a program allowing to make some figures and statistics.
In this blog post, I will detail the first results I obtained (and will update this post according to my progress).

!!!info "Note"
    You can recreate all the plots with updated information. To do so, you can check out the project on GitHub at [badaboom](https://github.com/vroger11/badaboom). I also use this repository as a template for my Python projects. Feel free to take a look and share any suggestions for improvement!

## Results

All the results that follow are calculated for the years between 1980 and 2030. Knowing that the data was retrieved on September 10, 2021, all data from that date to the end of 2030 are NASA's projections.

Before starting, here are the 10 largest asteroids for the years between 1980 and 2030:

| Asteroid identifier | Asteroid name | Estimated minimum diameter (km) | Estimated maximum diameter (km) | [Absolute magnitude h](https://ssd.jpl.nasa.gov/?glossary&term=H) | Asteroid information link |
|:----------------------------|:----------------------------|-------------------------:|-------------------------:|-----------------------:|:----------------------------------------------|
|                     2001036 | 1036 Ganymed (A924 UB)      |                 37.5452  |                  83.9537 |                   9.25 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2001036> |
|                     2000433 | 433 Eros (A898 PA)          |                 21.8049  |                  48.7573 |                  10.43 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2000433> |
|                     2001866 | 1866 Sisyphus (1972 XA)     |                  8.64082 |                  19.3215 |                  12.44 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2001866> |
|                     2004954 | 4954 Eric (1990 SQ)         |                  8.06408 |                  18.0318 |                  12.59 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2004954> |
|                     2001627 | 1627 Ivar (1929 SH)         |                  7.7724  |                  17.3796 |                  12.67 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2001627> |
|                     2003552 | 3552 Don Quixote (1983 SA)  |                  6.80072 |                  15.2069 |                  12.96 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2003552> |
|                     2002212 | 2212 Hephaistos (1978 SB)   |                  5.73528 |                  12.8245 |                  13.33 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2002212> |
|                     2025916 | 25916 (2001 CP44)           |                  4.88152 |                  10.9154 |                  13.68 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2025916> |
|                     2001980 | 1980 Tezcatlipoca (1950 LA) |                  4.61907 |                  10.3286 |                  13.8  | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2001980> |
|                     2005587 | 5587 (1990 SB)              |                  4.57673 |                  10.2339 |                  13.82 | <https://ssd.jpl.nasa.gov/sbdb.cgi?sstr=2005587> |

Thus, we realize that gigantic asteroids pass over our heads without us even realizing it!
Some of you will notice that some of the asteroid names are familiar (especially if you watch series like The Expanse).

Now let's take a look at the number of asteroids visiting us per month (the figure is interactive so feel free to move your mouse over the bars):

--8<--
assets/interactive_dataviz/badaboom/asteroids/fig1.html
--8<--

By seeing this graph we can draw the following points:

- The more the years pass and the more asteroids are observed (probably linked to an improvement of our observatories, even if more asteroids can cross us these last years).
- The number of predicted asteroid passes is lower than the last 5 years. This is perhaps due to the fact that the efforts are put on the largest asteroids (see the following figure which can suggest this).
- Asteroids do not usually make more than one pass per year.

Now let's go into more detail with the following figure (also interactive):

--8<--
assets/interactive_dataviz/badaboom/asteroids/fig2.html
--8<--

The asteroid categories are completely arbitrary. I can visualize these different sizes with these categories.
We notice that we detect more and more asteroids whose size is less than 500m. While when we look at the forecasts we see that the number expected is much less. Probably because the largest asteroids are a priority for the surveillance around our planet.

In addition to this graph, I calculated the following numbers:

- Single asteroid encountered and observed from 1980 to 2021-09-10: 25810
- Unique asteroid whose encounter is planned between 2021-09-10 and 2030: 11211

There are still quite a few visits planned, hoping that the biggest ones do not come into contact (for the most anxious, it is not on the menu).

That's it for today, I made this project for my curiosity and I thought I could share it with you.

I hope this helps/inspires some of you.
In any case, I hope it was interesting.

See you again, Vincent.
