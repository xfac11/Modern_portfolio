---
author: "Filip Karlsson"
title: "Bug Invasion"
date: 2021-12-01
description: "Third person shooter made in C# using Unity where I made an enemy with procedural animation"
tags: ["emoji"]
thumbnail: /oil_spillage_head.PNG
---

{{< youtube PP9p77pqGvc >}}

Bug invasion was part of GitHub Game Off 2021 which is a Anual game jam where you get one month to create a game based on one theme. The theme this year was "Bug" so the first thing I did was to brainstorm ideas. After removing "ant simulation" and "fly tapper" I landed on a game where the bugs in a computer is getting destroyed by the player or "fixed". I chose the Unity game engine because of my past experience with it.

The game is a third person shooter with a pistol to "kill" the bugs and a "fixer" to fix the glitches. I used Trello to plan the project and to manage all the tasks that needed to be done. With this project I wanted to tackle something new and finish in time for the jam. The new thing was procedural animation that I used for the bugs and player. 

## Procedural animation
My first step was to research procedural animation and ways of solving it. To lower the code and research needed I downloaded an already working inverse kinematics asset called Fast IK by [Daniel Erdmann](https://assetstore.unity.com/publishers/30624)


## Particle system
The system needed to be on the GPU for the fast performance and to use the GPU for this I needed to learn how to use compute shaders. For this
I had a book **Practical Rendering & Computation with Direct3D 1** that taught me how to use them and call them from the CPU side with DirectX11. The book also had
a section with particle systems which I could use for inspiration.

The particle system has three parts to it, **Add**, **Update** and **Rendering**. The image below shows the whole process of the system. First frame we see the particles getting added to the first buffer and then updated and placed in the second buffer. Lastly the partciles are rendered. The next frame the buffers switches places and repeats the previous steps. 
{{< figureSC partikelSystemet.png >}}
### Add
Add dispatches an compute shader which uses a constant buffer that has the initial values for the particles position, direction and a vector with random values. The random vector is used for a generated direction which makes each add more random. The total lifetime for the particle is position w/a value. This is then added to the AppendBuffer and repeated for each particle added.

### Update
The physics is done in this part and uses a compute shader that has two buffers, one append and one consume. Deltatime is added to the particles lifetime and if the lifetime is less or equal to zero we don't append it and it dies. If lifetime is more than zero we start to calculate the gravity force applied to it and each particle has a mass and gravity value to make it more flexible. **Vectorfields** are also used for a more lively particle where a size and force value are used. Force to manipulate how much the field should influence the particle and size for how big or small the field should be. These two forces are then added to the velocity of the particle and then the velocity is added to the position times delta time.

### Rendering
At first the particle system used geometry shader to generate the vertices but because the geometry shader is known to be slow I decided to remove it and only use Vertex and Pixel shader. Three vertices are used to create a triangle that gets rotated each frame. This creates the illusion of a star at 30 fps with big particles but when the particle is small it looks like a circle. I saw this approach on [Edan Kwans](https://edankwan.com/) the spirit, a web based particle system. Instanced rendering is used and necessary because the particles are on the GPU. Four different colors can be sent to the shader to be blended between eachother with the particles lifetime. Start and end size is also sent in this part and used with the lifetime to scale the particle.
{{< figureSC partikelTriangle.png >}}

### Reflection
This particles system was a fun and challenging task that required me to learn new techniques. Compute shaders, compute pipeline, different buffers and indirect instanced rendering to mention some. Communication and feedback from the team was important for the development. I could use that to fix bugs and create new functionality that team members found or wanted. For example when we added snow to a biome one member said that the snow didn't move much. They mentioned vector fields and after researching it I implemented it and showed it again. This constant feedback was used through the whole project and not unique to the particle system. I would improve on this system by adding texture support and blur. When playtesting one tester thought that the flame effect (which used one particle system) looked like slush. By using animated textures with the particle system this could be improved. To determine collision with the ground a y value was used. An improvement to this is to also enable collision with objects.

