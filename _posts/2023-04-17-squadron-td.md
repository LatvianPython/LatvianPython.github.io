---
layout: post
quote: how I play Squad TD 👾
title: Squadron TD
---


Around 3 years ago a couple of my friends got me into playing Squadron TD, 
Squadron Tower Defense (Squad TD) is a 1-8 player Starcraft 
II custom map focused around building defences and resource management 
[[1](https://squadtd.fandom.com/wiki/Squadtd_Wiki)].


<br/>
Gameplay video, if you do not know about the custom map:
YouTube: [Squadron Tower Defense Gameplay](https://www.youtube.com/watch?v=E75qtsaHwJ0)


<br/>
Problem with the custom map was the number of different towers you can elect to build, 
there were 11 different builders, each with their own sets of towers. Each tower can also be upgraded.

<br/>
We were also playing Chaos mode, which meant that on each wave you have a new random builder.

<br/>
Also, between waves, you have 35 seconds to build for the next wave of enemies.

<br/>
So, in 35 seconds, you have to:
1. figure out which builder you have
1. check what towers are available
1. find out what each tower does, and what it upgrades to using tooltips
1. decide which tower, or set of towers to build
1. place them down in a good location (depends on the tower type)

<br/>
This is too much to do in 35 seconds, it was not possible for me, a novice in the custom map, to 
be able to keep up.

<br/>
So, I wrote a tool that helps me with steps, because I am too lazy to learn all the builders, and towers.
I will not be beaten by a video game, I have the technology to beat it instead!

<br/>
Luckily for me, the Squadron TD community has all the available towers in a machine-readable format available on their 
[Reddit Wiki page](https://reddit.com/r/SquadronTowerDefense/wiki/versions)

<br/>
Using the CSVs provided by the community, and some OpenCV trickery, I was able to have a Python script that does
all the thinking for me from step 1 through 3. Step 4 I still have to do some thinking, but it is usually
just pick the tower that has the best relative damage or health values, depending on what the current build lacks, 
damage or tanking.

<br/>
With OpenCV, I find out how many resources I have, and what is the current builder.
Then with the list of builders available from Reddit, I can display a pretty table with colours
that allow me to choose the best tower that I can afford at the moment.

![squadron-td](/assets/images/squadron-td.png){: width="90%" }

<br/>
I have posted the source code for the script on my [GitHub](https://github.com/LatvianPython/squadron_td_helper)

<br/>
Coding it was half of the fun for me with the custom map. Do not really play it anymore, just those 3 years ago.
Was also a problem for me to get it running for the above screenshot, but it is cool that it still works, though
I did have to make minor adjustments, and do a little debugging.
