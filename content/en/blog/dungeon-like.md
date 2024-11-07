---
author: "Filip Karlsson"
title: "Dungeon-Like"
date: 2023-10-18
description: "Topdown 2D bullet heaven with many different items and weapons. Item system with various attacks and stats created with Godots' Resource, UI created with Godots' theme system, finite state machine for enemy movement, save and load system using JSON and Resource"
tags: ["Godot", "GDScript"]
thumbnail: /dungeon_like_head.png
github: "https://github.com/xfac11/Dungeon-like"
---


Dungeon like is a project I started after learning some Godot. I wanted to create a fully featured game with Godot so I picked a game genre that I felt had a small enough scope for me to complete. Vampire survivors and Brotato were very popular at the time so I decided to use them as inspiration for my game. 

The game is a Topdown 2D bullet heaven where I chose to use bought assets to dedicate the time to using the Godot engine and not creating the art myself. World and character art made by Buch and item(weapons, books etc) art made by Joe Williamson.

## Projectiles

One type of item is a weapon which shoot projectiles and I knew early that I needed a dynamic structure so I could create different behaviours. To create different type of projectiles a base class is used with defined and declared default functions that can be overwritten to create different behaviours. With GDScript it is possible to overwrite any parents' function but to enforce some kind of rule I chose to use upper case letters for those that are supposed to be overwritten. Hitbody, Movement and LifetimeEnd is the three functions that can be overwritten to create projectiles that behave differently.
![Dungeon Like projectiles](/DungeonLikeProjectileShowcase.gif)
### Moving
Every physics process the projectile moves using the Movement function that can be overwritten to create different type of movement. The default projectile moves by adding a normalized vector multiplied by a scalar speed to the position of the node. This is used for the knife item that shoots a knife from the player toward a random direction. The axe has a projectile that has a curve movement, the garlic has a projectile that stays on the player by setting its position to the player and the rotating projectile rotates around the player when shot.

### LifetimeEnd
The projectile has a lifetime and when it reaches its lifetime it gets destroyed. This is the default behaviour but with the garlic it is overwritten to instead cause damage each lifetime second.

### Hitbody
Hitbody is called when another body enters the Area2D node which is the root node of the Projectile. For this a signal is used called body_entered which the Area2D defines. The base projectile has a function that body_entered calls and in this the Hitbody is called with the body from the body_entered signal. With this we can cause damage to the enemy when it enters the area of the projectile. The spear uses a piercing projectile which keeps the number of hits as a integer and is destroyed when the number of hits is equal to the maximum number of hits. Another projectile is a on hit projectile which spawns four projectiles when being hit.

## Items
Items can be defined in the Godot editor by creating a Item resource. The Items are placed into the Items folder and the game will create a database of those items. The item has a name, description, texture, projectile scene and an item stat. 

The most interesting is the item stat which is also a resource that holds data for:
- Damage
- Speed
- Health
- Health regen
- Critical chance
- Critical damage
- Dodge chance
- Stackable
- Max stacks
- Projectile Lifetime
- Projectile speed
- Damage type
- Shooting type
- Item type

Shooting type is how the projectile is shot. Random dircetion, rotates around the player, stays on the player and only shoots once and more ways and is an enum. The item type is either weapon or passive to tell if it should add to the players' stats or be used as a weapon.

Each time an item is chosen to be added to the inventory an event/signal is fired that the inventory has been changed. The event sends the new item name and how many stacks there is. Then it checks if the new item is a passive and if so add the stats of the item to the player with the updated stack size. 
If it is a weapon it is handled each second by a function called ProcessItems which sends the weapon and the number of stacks to a different class called ItemHandler which shoots the projectile in the item with the shoot type of the weapon.