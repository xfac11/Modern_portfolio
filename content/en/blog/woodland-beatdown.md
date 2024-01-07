---
author: "Filip Karlsson"
title: "Woodland beatdown"
date: 2019-06-03
description: "2.5D animal fighter made in C++ using the DirectX11 graphics API where I made a deferred renderer with volume lights and more"
tags: ["emoji"]
thumbnail: /woodland_beatdown_head.png
---

{{< youtube JDSuGOHlfNI >}}

In this project we were 6 people in total and were tasked to create a game with DirectX 11 and C++. We landed on animal fighter with a gun-game mechanic. Each time you defeat an opponent you switch to the next animal. Having the last animal and defeating someone makes you the winner. We had our own group room where we could meet and work together and plan the game and choose what each member should do.

I chose four different techniques to implement into the game engine. Deferred rendering with light volumes, glow effect, shadow mapping, and skybox. I first implemented a forward renderer so others could test their techniques and the game. Deffered rendering is a different way of rendering opposite to forward rendering when all the objects are rendered directly to the back buffer. Instead in deffered rendering we split the rendering in two steps where the first step is to render to a g-buffer that contains different textures each containing data for the object. The second step renders each light using light volumes and using the data from the g-buffer to draw the pixel.

## Deffered rendering
The first step is to create the G-buffer which holds the different textures. In my implementation it has four textures which are both render targets and also shader resources for the second step. Render to these four textures with normal color, world position, texture color and glow map color. Below is an image captured with renderdoc displaying each texture.
{{< figureSC G-buffer.png >}}

After these textures are rendered they can be used as shader resources for the light part. Each point light is rendered using a sphere model which is scaled by the radius of the light and direction light is rendered using a fullscreen quad. The light part has two passes, one where it only renders front faces and writes to the stencil texture for every visible pixel and the second where it uses the stencil and depth texture to render the pixels inside the sphere.

An issue is rendering transparent objects. It is impossible with deferred rendering so to solve this I added an extra array which contains the transparent objects and then use forward rendering after the deferred part. Another issue was the performance of the game. In testing it showed that forward rendering was faster than deffered rendering. (I think the amount of lights used in the game wasn't enough to gain the benefits of using deffered rendering.)

An upgrade to this implementation would be to use a third part resulting in three parts. This method is called deffered lighting and is broadly used in modern engines. This also solves the issue with only having one material in deffered rendering. 
{{< figureSC WoodlandBeatdownLightVolume960x600.png >}}
## Shadow mapping
This method will render the models from the perspective of the light and then use this z-buffer when rendering to the scene. Updating the z-buffer only requires the vertex shader to be set in the pipeline so no pixel shader is needed. To render from the light, the view matrix uses the light direction and position and for the projection matrix an ortho matrix is used because the light is directional (sun) and will have the same direction for every light ray. When rendering the scene we use the model in world space and multiply it by these two martices and check if the z-coordinate is lower than the z-buffer. If it is then the pixel is in shadow. 

Some known issues with shadow mapping that came up while developing are shadow acne, peter panning, and overshadowing. Shadow acne is when the pixel is determined to be in shadow when it is not. This was solved by using a bias and subtracting the z-cordinate with this bias. Peter panning becomes a problem when the bias is set to high. This will detach the shadows from the object and will make it appear floating. To solve this we can tighten the far and near plane for the scene but keep the models in the shadow map. I solved it by testing different values for the far and near plane to get the best values. Overshadowing is when the scene is huge and the shadow map doesn't cover the whole scene. The outside of the shadow map will be shaded and to fix this a clamp sample is used when sampling the z-buffer and then check if the models z-coordninate is bigger than 1 