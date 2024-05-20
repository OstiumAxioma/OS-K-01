---
date: 2024-05-13
tags:
  - 游戏开发
  - 游戏开发/引擎
toolSet:
  - "[[OpenGL渲染管线]]"
knowledgeLink: 
status: true
---
# Unity渲染管线简介
```embed
title: "选择和配置渲染管线和光照解决方案 - Unity 手册"
image: "https://docs.unity3d.com/cn/2022.3/uploads/Main/BestPracticeLightingPipeline3.svg"
description: "本指南是以下 Unity 博客文章的更新版本：聚光灯团队最佳实践：设置光照管线 (Spotlight Team Best Practices: Setting up the Lighting Pipeline) - Pierre Yves Donzallaz。"
url: "https://docs.unity3d.com/cn/2022.3/Manual/BestPracticeLightingPipelines.html"
```

渲染管线执行一系列操作来获取场景的内容，并将这些内容显示在屏幕上。概括来说，这些操作如下：

- 剔除
- 渲染
- 后期处理
不同的渲染管线具有不同的功能和性能特征，并且适用于不同的游戏、应用程序和平台。

将项目从一个渲染管线切换到另一个渲染管线可能很困难，因为不同的渲染管线使用不同的着色器输出，并且可能没有相同的特性。因此，必须要了解 Unity 提供的不同渲染管线，以便可以在开发早期为项目做出正确决定。
## 选择要使用的渲染管线
Unity 提供以下渲染管线：

- 内置渲染管线是 Unity 的默认渲染管线。这是通用的渲染管线，其自定义选项有限。
- 通用渲染管线 (URP) 是一种可快速轻松自定义的可编程渲染管线，允许您在各种平台上创建优化的图形。
- 高清渲染管线 (HDRP) 是一种可编程渲染管线，可让您在高端平台上创建出色的高保真图形。
- 可以使用 Unity 的可编程渲染管线 API 来创建自己的自定义渲染管线。
## 管线的基本概念
首先，让我们来看看在本文中经常遇到的几个重要图形渲染术语的定义。

渲染管线确定场景中对象的显示方式，分为三个主要阶段。
1. 剔除：它列出了需要渲染的对象，最好是那些对摄像机可见的对象（视锥体剔除）和其他对象不遮挡的对象（遮挡剔除）。
2. 渲染：是指将这些对象绘制到基于像素的缓冲区中（通过正确的光照以及它们的一些属性）。
3. 后处理：最可以在这些缓冲区上执行后期处理操作，例如，应用颜色分级、泛光和景深，从而生成发送到显示设备的最终输出帧。

![[Pasted image 20240514180809.png|center|500]]

其中，着色器是在图形处理单元 (GPU) 上运行的程序或程序集合的通用名称。例如，在剔除阶段完成后，顶点着色器用于将可见对象的顶点坐标从“对象空间”转换为称为“裁剪空间”的不同空间；然后 GPU 使用这些新的坐标对场景进行光栅化，也就是将场景的矢量表示转换为实际像素。在稍后阶段，这些像素将由像素（或片元）着色器进行着色；像素颜色通常将取决于各自表面的材质属性以及周围的光照。现代硬件上另一种常见的着色器是计算着色器：计算着色器允许程序员利用 GPU 的大量并行处理能力，用于任何类型的数学运算，如光照剔除、粒子物理或体积模拟。

下面的流程图从内容创建者的角度，大致显示了 Unity 中的整个光照管线

![[Pasted image 20240514180958.png|center]]

首先要选择一个渲染管线。然后决定如何产生间接光照，并相应地选择一个全局光照系统。确保为您的项目适当地调整了所有全局光照设置之后，您可以继续添加光源、发光表面、反射探针、光照探针和光照探针代理体 (LPPV)。

# 材质系统

要在 Unity 中绘制某物，您必须提供描述其形状的信息以及描述其表面外观的信息。使用网格可描述形状，使用材质可描述表面的外观。

材质和着色器紧密相连；总是通过着色器使用材质。材质包含对 Shader 对象的引用。如果 Shader 对象定义材质属性，则材质还可以包含数据（如颜色或纹理参考等）。

基于Unity渲染管线的三个阶段，Unity材质包含四种渲染模式：
- Opaque
	- Suitable for normal solid objects with no transparent areas
- Cutout
	- Allows you to create a transparent effect that has hard edges between the opaque and transparent areas.
- Transparent
	- Suitable for rendering realistic transparent materials such as clear plastic or glass
- Fade
	- Allows the transparency values to entirely fade an object out, including any specular highlights or reflections it may have
