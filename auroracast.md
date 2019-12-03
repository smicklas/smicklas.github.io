---
layout: post
title: AuroraCast
description: 
image: assets/images/auroracast_top.png
nav-menu: true
---

Going to school in the North meant that we would get occasional glimpses at the Northern Lights. However, I always struggled to be able to quickly parse through the reports to tell if I would be able to see them - I didn't want to have to remember my longtitude and latitude, figure out the Kp index at my location, and have to check for cloud coverage.
<br><br>
Instead, using Darksky API and aurora tabular reports from the NOAA, I created a better visual indicator for visibility.
<br><br>
The cloud animation grows in opacity with real location cloud coverage. The glow on the circle represents the Kp index, or the measure of magnetic field disruptance caused by solar wind. Here are a couple of the different views:
<br>
<div style="text-align: center; border-top:none;">
    <img src="assets\images\aurora_1.png" alt="" width="350" height="403"/>
    <img src="assets\images\aurora_2.png" alt="" width="350" height="403"/>
    <img src="assets\images\aurora_3.png" alt="" width="350" height="403"/>
</div>
<br>
It is built with HTML, CSS, and Javascript.  
<br>

A current side project is re-factoring it into React. Below is a mockup built with Adobe Xd.

<div style="text-align: center; border-top:none;">
    <img src="assets\images\aurorareact_1.png" alt="" width="50%" height="50%"/>
    <img src="assets\images\aurorareact_2.png" alt="" width="50%" height="50%"/>
</div>
