---
date: 2024-05-14
tags:
  - 游戏开发
  - 游戏开发/引擎
toolSet:
  - "[[102-Computational Geometry in a Physics Engine]]"
knowledgeLink: 
status: true
---
物理引擎（Physics Engine）是一个软件组件，它将游戏世界对象赋予现实世界物理属性（重量、形状等），并抽象为刚体（Rigid）模型（也包括滑轮、绳索等），使得游戏物体在力的作用下，仿真现实世界的运动及其之间的碰撞过程。即在牛顿经典力学模型基础之上，通过简单的 API 计算游戏物体的运动、旋转和碰撞，现实的运动与碰撞的效果。

使用物理引擎，游戏开发者仅需考虑给游戏物体赋予形状（假设为均匀分布）和力，在游戏引擎驱动下自动完成运动与碰撞的计算。

## 游戏世界运动分类

深入了解Unity物理系统前，我们需要对游戏的物理有基本的了解。游戏中的物理大致有以下分类：
### 运动学（Kinematics）
从几何的角度（指不涉及物体本身的物理性质和加在物体上的力) 描述和研究物体位置随时间的变化规律的力学分支。点的运动学研究点的运动方程、轨迹、位移、速度、加速度等运动特征，以及在不同空间中的变换。运动学是理论力学的一个分支学科，它是运用几何学的方法来研究物体的运动。

- 不考虑外部力作用下的运动
- 将一个物体作为几何部件，抽象为质点运动模型
- 仅考虑物体位置、速度、角度 …
- 研究方法：代数，如：线性代数
### 动力学（Dynamics）
它主要研究作用于物体的力与物体运动的关系。动力学的研究对象是运动速度远小于光速的宏观物体。动力学的研究以牛顿运动定律为基础；牛顿运动定律的建立则以实验为依据。动力学是牛顿力学或经典力学的一部分，但自20世纪以来，动力学又常被人们理解为侧重于工程技术应用方面的一个力学分支。

在游戏物理引擎中，主要是<mark class="hltr-orange">刚体动力学</mark>。主要包括质点系动力学的基本定理，由动量定理、动量矩定理、动能定理以及由这三个基本定理推导出来的其他一些定理。动量、动量矩和动能（见能）是描述质点、质点系和刚体运动的基本物理量。

- 考虑外部力对物体运动的影响
- 包括重力、阻力、摩擦力等，以及物体的重量和形状，甚至弹性物体
- 通常将一个物体当作刚体
- 模拟物体在现实世界中的运动
## Unity物理引擎基础知识
### 物理引擎学习与使用

物理引擎的使用可能是最简单的。你要做的事情可能就是将力作用在游戏物体上。

尽管你不必学习物理引擎原理与算法，如
- 物理引擎涉及复杂的运动学知识
- 碰撞计算与优化

> [!tip] 底层物理
> 这部分知识你可以在[[101-Introduction to Computational Geometry|计算几何]]中找到

但为了有效使用物理引擎，你需要：
- 了解物理运动的基本知识
- 理解游戏离散仿真产生的特殊现象，避免游戏失真
- 了解可能导致性能问题的方面，使得游戏更加流畅

### 运动与物体建模

#### **刚体（Rigid body）**

刚体是指在运动中和受力作用后，形状和大小不变，而且内部各点的相对位置不变的物体。

- 绝对刚体实际上是不存在的，只是一种理想模型，因为任何物体在受力作用后，都或多或少地变形，如果变形的程度相对于物体本身几何尺寸来说极为微小，在研究物体运动时变形就可以忽略不计。
- 齿轮、绳索、滑轮不属于刚体，但属于该引擎。因此：物理引擎不只是刚体
$$

$$
#### 运动的点模型
将物体的运动作为一个点，则一个如图物体
![[Pasted image 20240517183607.png|center|]]
- 具有质量
- 具有中心
- 具有质心（不考虑形状）
	- 假设1：物体是均质的
	- 假设2：中心与质心重合
- 物体作用力分解为：
	- 作用中心点上的力（Force）
	- 围绕中心点旋转的力矩（Torque）

当有形状的物体发生碰撞时：

- 使用点模型
- 必须计算在哪个点产生碰撞，并计算力和力矩

> [!important]
> 可计算碰撞的物体必须是 （[[103-Convex Hull 凸包]]）物体
> 必须是三角形表示的多面体（三角网格）

2D：物体分解为若干三角形
3D：凸物体表面用三角形，分解为四面体表示

> [!tip]
> 如果是 Concave 物体
> 必须分解为多个凸物体组合

## Unity 物理引擎实现
为了拥有令人信服的物理行为，游戏中的物体必须正确加速，并受到碰撞、重力和其他力的影响。Unity的内置物理引擎为您提供了处理物理模拟的组件。只需几个参数设置，您就可以创建以现实方式被动行为的对象(例如，它们将被碰撞和跌落移动，但不会自己开始移动)。通过从脚本中控制物理，您可以为对象提供车辆、机器甚至一块织物的动态。

Unity中的物理引擎由两个重要组件组成：
- Rigidbody<mark class="hltr-orange">刚体模拟</mark>
- Collision<mark class="hltr-cyan">碰撞箱</mark>

其中**刚体模拟**负责调整物体作为一个点模型的所有属性，包括质量、重力、角速度、加速度等

**碰撞箱**负责进行碰撞检测

### 刚体模拟
Rigidbodies enable your GameObjects to act under the control of physics. The Rigidbody can receive forces and torque to make your objects move in a realistic way. Any GameObject **must contain a Rigidbody** to be influenced by gravity, act under added forces via scripting, or interact with other objects through the NVIDIA PhysX physics engine.

![[Pasted image 20240517184419.png|center]]

Kinematic Rigidbody是一种特殊的刚体，不会对任何物理效果做出反应，其作用主要是作为触发器使用
### 碰撞箱
Collidor的类型有很多种，使用不同的距离检测方法判断是否有碰撞，比如圆形会计算圆心的半径内有没有物体。只有[[103-Convex Hull 凸包#Convex Hull Algorithm]]碰撞箱是例外。

> [!important]
> 对于不规则的物体，我们首先应考虑使用基础碰撞箱拟合其形状，最后考虑凸包。这样对性能的要求最小，因为凸包的碰撞检测需要遍历整个凸包。

和键盘输入一样，碰撞检测的C#函数也包括了三种基本状态：
- OnCollisionEnter
	- Called when the colliders intersect for the first time
- OnCollisionStay
	- Called during updates where contact is maintained
- OnCollisionExit
	- Called when the contact is broken (no longer colliding)

> [!success]
> 同时，碰撞箱是触发器的基础条件，因为它可以检测碰撞后发出布尔值来通知其它组件。

触发器同样也有三种基本状态：
- OnTriggerEnter
	- Called when the colliders intersect for the first time
- OnTriggerStay
	- Called during updates where contact is maintained
- OnTriggerExit
	- Called when the contact is broker (no longer colliding)