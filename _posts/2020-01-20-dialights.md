---
layout: post
title: DiaLights
date: 2020-01-20 23:18 +0800
feed-img: ../assets/images/dialights_cover.png
description: RBG lights controlled by Arduino, bluetooth, and Android application
category: [project]
---
<img class="pink-shadow center" src="/assets/images/dialights.png" alt="DiaLights on the wall"/>

A personal project inspired by popular lighting piece Nanoleaf, DiaLights are RGB wall art panels. The physical component, the panels themselves, are composed of an Arduino Nano, a DF Robot Bluetooth module, strip RGB LEDs, and 3D printed frames. The frames were modelled using AutoDesk AutoCad. 

The app was developed using the Android SDK in Java. The Arduino code is written in C++. The app sends serial data to the Bluetooth module, connected to a data pin on the Arduino.

<div class="siema">
    <img class="w-60" src="/assets/images/dialights_app_1.png" alt="Screenshot of DiaLights app"/>
    <img class="w-60" src="/assets/images/dialights_app_2.png" alt="Screenshot of DiaLights app"/>
    <img class="w-60" src="/assets/images/dialights_app_3.png" alt="Screenshot of DiaLights app"/>
</div>

<div class="gallery-button-container center">
    <button class="prev center gallery-button"><i class="fas fa-backward" aria-hidden="true"></i></button>
    <button class="next center gallery-button"><i class="fas fa-forward" aria-hidden="true"></i></button>
</div>


The individual panel colors can be controlled individually, synced to one color, turned on and off, or set to slowly pulse through a range of colors.

Additional information and code can be found on the <a href="https://github.com/smicklas/DiaLights">Github repository</a>.