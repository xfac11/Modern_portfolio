---
author: "Filip Karlsson"
title: "Dungeon-Like"
date: 2023-10-18
description: "Topdown 2d bullet heaven inspired by Vampire Survivors"
tags: ["Godot", "GDScript"]
thumbnail: /dungeon_like_head.png
---

```python
//example class
public class Health : MonoBehaviour
{
  private int currentHealth;
  private int maxHealth;
  public void Damage(int damage)
  {
    OnDamage?.Invoke();
    currentHealth -= damage;
    if(currentHealth <= 0)
    {
      OnDeath?.Invoke();
    }
  }
}
```
