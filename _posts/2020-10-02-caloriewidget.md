---
layout: post
title: Calorie Tracker
date: 2021-02-27 23:18 +0800
tags: [design, development, projects]
toc:  true
feed-img: ../assets/images/calorie_cover.png
description: Hydrid Tizen application for tracking and calculating calories
---

At the beginning of 2021, I was looking to upgrade from my old Fitbit Charge 2. One of the candidates was the Samsung Active Watch 2 - it was beautiful, the app looked great, and overall seemed to be one of the best new fitness-focused smartwatches. When I ordered it, I noticed one of my favorite features from the Fitbit was gone - a calorie tracker! 

After spending some time looking through third party widgets, I wasn't able to find a good calorie tracker. So, I set out to make my own! Besides, how hard could calculating calories based on heart rate be?

I was <strong>very</strong> naive. It was not simple. The development of this app was a comedy of errors, but a great learning experience for wearable development, particularly within the Tizen environment. 

First I started off with the easy part - designs! I created a couple different versions of the widget UI. I wanted to somewhat mimic the existing Samsung Health app UI. In order of the slideshow, they are: 

- Calories burned & calories consumed 
- Calories burned & steps 
- Calories burned by activity (resting, running, elliptical, etc)
- Detailed/swiped view of calories burned by activity 

<div class="siema">
    <img src="../../../../assets/images/calorie_1.png" alt="AuroraCast screenshot"/>
    <img src="../../../../assets/images/calorie_2.png" alt="AuroraCast screenshot"/>
    <img src="../../../../assets/images/calorie_3.png" alt="AuroraCast screenshot"/>
    <img src="../../../../assets/images/calorie_4.png" alt="AuroraCast screenshot"/>
</div>
<div class="gallery-button-container center">
    <button class="prev center gallery-button"><i class="fas fa-backward" aria-hidden="true"></i></button>
    <button class="next center gallery-button"><i class="fas fa-forward" aria-hidden="true"></i></button>
</div>

After, I spent a good amount of time devising an algorithm to calculate calories per minute based on height, weight, age, gender, and heart rate. It looked something like this, and wasn't too far off from what my Fitbit would end up with: 

{% highlight js %}
if(female)
    basal_metabolic_rate_per_min = ((10 * weight_in_kgs) + (6.25 * height_cm) - (5 * age) - 161)/1440;
if(male)
	basal_metabolic_rate = ((10 * weight_in_kgs) + (6.25 * height_cm) - (5 * age) + 5)/1440;

if(heart_rate <= heart_rate_resting)
    return calories_per_min = basal_metabolic_rate_per_min;
else if(heart_rate > 89 && heart_rate < 151)
    if(female)
        return calories_per_min = (-20.4022 + (0.4472 * hr) - (0.1263 * weight_in_kgs) + (0.074 * age)) / 4.184;
    if(male)
        return calories_per_min = (-55.0969 + (0.6309 * hr) - (0.1988 * weight_in_kgs) + (0.2017 * age)) / 4.184;
else
    calories_per_min = (0.049 * hr - 1.5) * 3.5 * weight_in_kgs / 200;
{% endhighlight %}

It breaks down to about three important formulas. The reason for this is heart rate vs calories burned a minute is not linear. Each formula used is optimized for a specific heart rate range. At rest,  a human will be using only their basal metabolic rate, or the amount of calories needed to keep the body functioning. The last formula is a tweaked version of one presented <a href="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6003065/">this </a> study, which attempts to correlate higher intensity excercise and METs. METs is a standardized measure of estimated intensity versus a resting heart rate. It then calculates calories per minute as a function of that estimated METs. 

The next step was to create the widget. In Tizen studio and Tizen development, you can create a view for the widget by just using HTML, CSS, and JS. I tested using a combination of remote building to my personal device and an emulator. 

However, when I started writing Javascript to track the heart rate from the watch's sensor, I noticed that as soon as you swiped from the widget away, it stopped running. Looking for solutions, I found that I may want to have a wearable widget + app combo. The app could run in the background, do the calculations for the calories based on heart rate, then tell the widget what to display. 

Surprisingly, that didn't work either. The application would automatically suspend after x minutes of inactivity from the user. 

The next step was to try a service. An application would be used to kick-off a service. A Tizen service would run in the background listening to the heart rate sensor and place the calculation data in a message channel. The widget would fetch that data from the channel when opened/swiped to by the user, and the data would be displayed. As a quick test I had the service increment an integer every couple of seconds, and the widget displayed it even after a couple of minutes when the app would time out before. Finally! It seemed like we had a solution! 

However, services don't have access to the Sensor APIs needed to listen to the heart rate. Even if they did, the test I outlined above drained the battery way too much to be feasible for the application. 

It was disappointing to have to call it quits on the project for now, but maybe in the future this will be added to Tizen. I guess I had to learn the hard way why there aren't any calorie trackers! 

If you'd like to see the code, <a href="https://github.com/smicklas/CalorieWidget">the repository is available here. </a>

(In the end, I returned that watch and got a Charge 4.)