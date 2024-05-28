---
date: 2024-05-22
tags:
  - 游戏开发
  - 游戏开发/引擎
toolSet: 
knowledgeLink: 
status: true
---
# Projectile
Projectile in FPS refers to the bullet objects that player will shoot. We need a projectile prefab to fulfill this need, which also contains collision detection and Rigidbody.

Because camera is what player see in the FPS game, we need to shoot this projectile from the camera as well.

- Instantiate the projectile prefab at camera location
- Apply a forward force to the projectile

This need us to do a [[Factory Mode 工厂模式|factory mode]] design pattern.

# Health System

![[02-Game_Developing/CS5540_GameProgramming/CS5540_System.excalidraw.md#^area=jFvWLivIURQyJXKXljZPx|HealthSystem|center]]