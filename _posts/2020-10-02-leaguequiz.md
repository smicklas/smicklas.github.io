---
layout: post
title: League of Legends Ability Quiz
date: 2021-04-05 23:18 +0800
tags: [design, projects]
toc:  true
feed-img: ../assets/images/abilityquiz_cover.png
description: Quiz to test League of Legends champion abilities 
---

This one is a current work in progress! Right now the only functionality is scraping the champion abillity, image, name, key binding, and description from this site content: https://www.mobafire.com/league-of-legends/abilities and then displaying it in plain text. 

The plan is to then implement it into a randomly sorted quiz where users will guess what the name of the ability is. Below are some example mockups created in Figma. The repository can be found <a href="https://github.com/smicklas/LeagueAbilityQuiz"> here.</a>

<div class="siema">
    <img src="../../../../assets/images/AbilityQuiz-Main.png" alt="Mockup of League of Legends Ability Quiz"/>
    <img src="../../../../assets/images/AbilityQuiz-Incorrect.png" alt="Mockup of League of Legends Ability Quiz"/>
    <img src="../../../../assets/images/AbilityQuiz-Correct.png" alt="Mockup of League of Legends Ability Quiz"/>
</div>
<div class="gallery-button-container center">
    <button class="prev center gallery-button"><i class="fas fa-backward" aria-hidden="true"></i></button>
    <button class="next center gallery-button"><i class="fas fa-forward" aria-hidden="true"></i></button>
</div>

My goal was to cloosely mimic the style of the style used in the <a href="https://www.chiboost.net/upload/2019/03/IMAGE-FOR-LOL-CLIENT-SPEC-MODE-1024x576.jpg">League of Legends launcher</a>, <a href="https://images2.minutemediacdn.com/image/upload/c_fill,w_912,h_516,f_auto,q_auto,g_auto/shape/cover/sport/5afde7387134f68573000003.png">champ select</a>, and <a href="https://na.leagueoflegends.com/en-us/">website</a>. 