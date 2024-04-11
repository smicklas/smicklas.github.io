---
layout: post
title: League of Legends Ability Quiz
date: 2021-04-05 23:18 +0800
feed-img: ../assets/images/abilityquiz_cover.png
description: Quiz to test League of Legends champion abilities 
categories: [project]
tags: [frontend, react, design]
---

This project is a quiz game to test user's knowledge of League of Legends abilities developed with React. 

The champion ability, image, name, key binding, and description from this site content: <a href="https://www.mobafire.com/league-of-legends/abilities">https://www.mobafire.com/league-of-legends/abilities</a>. The data would get parsed with regexes to extract the needed parameters. 

This project, like many, started with some mockups - screenshots below were created in Figma. 

<div class="siema">
    <img src="/assets/images/AbilityQuiz-Main.png" alt="Mockup of League of Legends Ability Quiz"/>
    <img src="/assets/images/AbilityQuiz-Incorrect.png" alt="Mockup of League of Legends Ability Quiz"/>
    <img src="/assets/images/AbilityQuiz-Correct.png" alt="Mockup of League of Legends Ability Quiz"/>
</div>
<div class="gallery-button-container center">
    <button class="prev center gallery-button"><i class="fas fa-backward" aria-hidden="true"></i></button>
    <button class="next center gallery-button"><i class="fas fa-forward" aria-hidden="true"></i></button>
</div>

My goal was to closely mimic the style of the style used in the <a href="https://miro.medium.com/v2/resize:fit:4800/format:webp/1*NxkLhUAk5y5qiDRhdt89pQ.png">League of Legends launcher</a>, <a href="https://images2.minutemediacdn.com/image/upload/c_fill,w_912,h_516,f_auto,q_auto,g_auto/shape/cover/sport/5afde7387134f68573000003.png">champ select</a>, and <a href="https://na.leagueoflegends.com/en-us/">website</a>. 

It was briefly hosted on Heroku for other League of Legends players to play test. I wanted to reduce the client-side load, so I upgraded to use some backend services. When a user started up their game, the app would hit an AWS lambda to get the game data. 

Below are some screenshots of the final implementation. I added some loading, a game landing screen and a final ranking. 

<div class="siema">
    <img src="/assets/images/league-quiz-impl-1.png" alt="Landing screen of League of Legends Ability Quiz"/>
    <img src="/assets/images/league-quiz-impl-2.png" alt="Loading screen of League of Legends Ability Quiz"/>
    <img src="/assets/images/league-quiz-impl-3.png" alt="Game screen of League of Legends Ability Quiz"/>
    <img src="/assets/images/league-quiz-impl-4.png" alt="Game screen with incorrect guess of League of Legends Ability Quiz"/>
    <img src="/assets/images/league-quiz-impl-5.png" alt="Game screen with correct guess of League of Legends Ability Quiz"/>
    <img src="/assets/images/league-quiz-impl-6.png" alt="Final score screen of of League of Legends Ability Quiz"/>
</div>
<div class="gallery-button-container center">
    <button class="prev center gallery-button"><i class="fas fa-backward" aria-hidden="true"></i></button>
    <button class="next center gallery-button"><i class="fas fa-forward" aria-hidden="true"></i></button>
</div>

The projects needs a little TLC  - Mobafire has since updated their HTML which means many of the regexes no longer work. If you're still interested in taking a peek, <a href="https://github.com/smicklas/LeagueAbilityQuiz">the repo is here.</a> Some time in the future I'll need to either update the regexes, or just create a DB for the entries and groom them myself. 
