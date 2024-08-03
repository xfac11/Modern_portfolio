---
author: "Filip Karlsson"
title: "Bug Invasion"
date: 2021-12-01
description: "Third person shooter made in C# using Unity where I made an enemy with procedural animation"
tags: ["emoji"]
thumbnail: /oil_spillage_head.PNG
---

{{< youtube PP9p77pqGvc >}}

Bug invasion was part of GitHub Game Off 2021 which is a anual game jam where you get one month to create a game based on one theme. The theme was "Bug" so the first thing I did was to brainstorm ideas. After removing "ant simulation" and "fly tapper" I landed on a game where the bugs in a computer is getting destroyed by the player or "fixed". I chose the Unity game engine because of my past experience with it.

The game is a third person shooter with a pistol to "kill" the bugs and a "fixer" to fix the glitches. I used Trello to plan the project and to manage all the tasks that needed to be done. With this project I wanted to tackle something new and finish in time for the jam. The new thing was procedural animation that I used for the bugs and player. 

## Procedural animation
After watching [Codeers](https://youtu.be/e6Gjhr1IP6w?si=Iv_sTqm-Kx0YjVQV) brilliant video, I wanted my bugs to behave the same. To start off I used the inverse kinematics asset called Fast IK by [Daniel Erdmann](https://assetstore.unity.com/publishers/30624). It is used to determine the motion a bone structure should make to reach a certain position. For example in games where the foot of the character is placed on top of a staircase. In this case the position is determined by a ray shooting down from the foot and colliding with the scene, say the staircase.
In Bug Invasion the bug has 6 legs, 3 on each side. Each leg has a target transform that follows the body of the bug in relation to the leg. The targets' y position is set to the ground by using a ray and shooting it down from the leg. The foot is stuck to the ground until the target reaches a max distance from the foot. In the code this is checked by a boolean CanMove and is true when the max distance is reached. When the move is complete the CanMove boolean is set to false again. To move the foot from the stuck position to the target, a Vector3 lerp is used to smoothly move the leg over time. 

## Enemy AI using Unity Nav mesh agent

## VFX with Unity shader graphs and particle system

## Third person camera

## Shop UI