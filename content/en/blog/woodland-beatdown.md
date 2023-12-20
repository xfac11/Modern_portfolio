---
author: "Filip Karlsson"
title: "Woodland beatdown"
date: 2019-06-03
description: "2.5D animal fighter made in C++ using the DirectX11 graphics API where I made a deffered renderer with volume lights and more"
tags: ["emoji"]
thumbnail: /woodland_beatdown_head.png
---

{{< youtube JDSuGOHlfNI >}}

In this project we were 6 people in total and were tasked to create a game with DirectX 11 and C++. We landed on animal fighter with a gun-game mechanic. Each time you defeat an opponent you switch to the next animal. Having the last animal and defeating someone makes you the winner. We had our own group room where we could meet and work together and plan the game and choose what each member should do.

I chose four different techniques to implement into the game engine. Deferred rendering with light volumes, glow effect, shadow mapping, and skybox. I first implemented a forward renderer so others could test their techniques and the game. Deffered rendering is a different way of rendering opposite to forward rendering when all the objects are rendered directly to the back buffer. Instead in deffered rendering we split the rendering in two steps where the first step is to render to a g-buffer that contains different textures each containing data for the object. The second step renders each light using light volumes and using the data from the g-buffer to draw the pixel.

## Deffered rendering
The first step is to create the G-buffer which holds the different textures. In my implementation it has four textures which are both render targets and also shader resources for the second step. Render to these four textures with normal color, world position, texture color and glow map color. Below is an image captured with renderdoc displaying each texture.
{{< figureSC G-buffer.png >}}

After these textures are rendered they can be used as shader resources for the light part. Each point light is rendered using a sphere model which is scaled by the radius of the light. Direction light is rendered using a quad. 

