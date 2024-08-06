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

In Bug Invasion the bug has 6 legs, 3 on each side. For each leg there is a point transform that is a child of the bug and offset by some value to be close to the leg. Each point has a component with a script that sets the y position to the ground by using a ray and shooting it down from the leg. Each leg has a target transform with a component that stick the foot to the ground until the point reaches a max distance from the foot. The target is the certain position for the IK to solve a motion for. To move the foot from the stuck position to the point, a Vector3 lerp is used to smoothly move the leg over time. To move the foot up a bit the mathf function pingpong is used to move the target up and then down to the point. Without this the foot would move to the point gliding on the ground. 

{{< youtube yGbqtCQnr20 >}}
In the video above you can see the bug animated with a slower step time than in the finished game to show the different stages between not stuck and stuck.
The target is represented as a green or red sphere wireframe. When it is red it means it is stuck and green when not stuck. The blue one is the point.
## Enemy AI using Unity Nav mesh agent

## VFX with Unity shader graphs and particle system

## Third person camera

## Shop UI