---
layout: post
comments: true
title:  "My dataviz for The Grand Melee"
ref: tgm1
date:   2023-01-16 12:00:00 +0200
categories: blog dataviz aoe
category: blog
lang: en
---

This post is the third episode in my Age of Empires 2 tournament series (check out [my first](/blog/dataviz/aoe/2022/12/08/redbull-wololo-legacy) and [my second](/blog/dataviz/aoe/2022/12/13/warlords) if you missed them).
All the work done here is about the main event (that's what interests me the most in tournaments).
As a reminder, I published most of these visualizations on [Reddit](https://www.reddit.com/user/vroger11) before making this post. This one is a summary of the tournament and the feedback I got.

This time, I improved my visualizations with additional data from [Age of Empires Captains Mode](https://aoe2cm.net/) when it was useful and by adding a QR code as my signature for my works.

Now let's take a look at that work 😃.

# Player Achievements

My first visualization is the one showing each rank obtained from the players with the number of games they won, lost.
Here, there were fewer players for the main event, still we saw some great matches.

{:refdef : style="text-align : center ;"}
![Réalisations des joueurs](/assets/images/dataviz/aoe/tgm/1/rank_games.png){:width="1200px"}
{: refdef}

# Which civilization was played on each map?

Next, we have a sankey on which civilization was played on each map.
In this tournament, the civilizations that have not been played are: Celts, Chinese, Hungarians, Malays, Malians, Sicilians, Slavs, Spanish, Teutons, Vietnamese and Vikings.
I preferred the sankey to the heat matrix, as the matrix has many zeros. To be seen if sankey is more readable on events with more matches 😉


{:refdef : style="text-align : center ;"}
![Quelles sont les civilisations jouées sur chaque carte ?](/assets/images/dataviz/aoe/tgm/1/civ_v_map_sankey.png){:width="1100px"}
{: refdef}

# Which maps were popular?

Here I added the ban information from the player drafts.
I liked this, but I'd like to find a more suitable display for the future.

{:refdef : style="text-align : center ;"}
![Quelles sont les cartes les plus populaires ?](/assets/images/dataviz/aoe/tgm/1/maps_played.png){:width="1200px"}
{: refdef}

# Kill/Death of each civilization played

Here I added the snipe and ban information from the player drafts.
I still love the discussions this graph generates 😄.
I had some ideas to improve this graph (including having snipe/bans/death as negative values), we'll see at the next tournament if I get better visualizations.

{:refdef: style="text-align: center;"}
![Tué/mort pour chaque civilisation jouée](/assets/images/dataviz/aoe/tgm/1/civ_played.png){:width="1200px"}
{: refdef}

# The wheel of victory

Finally, the victory wheel!
Like my previous post, this is exclusive to those reading this post 😉.
You can hover over each segment with your mouse to gather information, and even click on the categories to examine a specific card or card category (click on the tournament logo to go back).
Please don't hesitate to give me tips on how to improve this work.

<div style="width : 1000px ; margin : 0 auto ;">
    {% include interactive_dataviz/aoe/tgm/1/wheel.html %}
</div>

# What is the next step?

I'm thinking about refining and reworking some of my graphs for the future.
You'll see that for the next tournament I cover.
By the way, it will be the T90 Titans League 2: Platinum League, the hype is on!

I hope this helps and/or inspires some of you.

See you again, Vincent.

