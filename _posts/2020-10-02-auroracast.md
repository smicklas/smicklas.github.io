---
layout: post
title: Auroracast
date: 2019-11-8 23:18 +0800
tags: [projects]
feed-img: ../assets/images/aurora_cover.png
description: Simple web page for predicting Northern Lights visiblity
---

<img class="pink-shadow center" src="../../../../assets/images/auroracast.png"/>

Going to school in the North meant that we would get occasional glimpses at the Northern Lights. However, I always struggled to be able to quickly parse through the reports to tell if I would be able to see them - I didn’t want to have to remember my longtitude and latitude, figure out the Kp index at my location, and have to check for cloud coverage.

Instead, using Darksky API and aurora tabular reports from the NOAA, I created a better visual indicator for visibility.

The cloud animation grows in opacity with real location cloud coverage. The glow on the circle represents the Kp index, or the measure of magnetic field disruptance caused by solar wind. Here are a couple of the different views:

<div class="siema">
    <img class="w-70" src="../../../../assets/images/aurora_1.png" alt="AuroraCast screenshot"/>
    <img class="w-70" src="../../../../assets/images/aurora_2.png" alt="AuroraCast screenshot"/>
    <img class="w-70" src="../../../../assets/images/aurora_3.png" alt="AuroraCast screenshot"/>
</div>
<div class="gallery-button-container center">
    <button class="prev center gallery-button"><i class="fas fa-backward" aria-hidden="true"></i></button>
    <button class="next center gallery-button"><i class="fas fa-forward" aria-hidden="true"></i></button>
</div>

It is built with HTML, CSS, and Javascript. The repository can be found <a href="https://github.com/smicklas/AuroraReact">here</a>.

As a excercise later on, I wanted to create an alternate layout for the information: 

<img src="../../../../assets/images/aurorareact_1.png" alt="AuroraCast screenshot"/>
<img src="../../../../assets/images/aurorareact_2.png" alt="AuroraCast screenshot"/>

Additional information and code can be found on the <a href="https://github.com/smicklas/DiaLights">Github repository</a>.