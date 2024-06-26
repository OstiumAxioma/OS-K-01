---
date: 2024-01-17
tags:
  - 计算机/编程语言
  - 计算机/图形学
  - 计算机/图形学/渲染管线
toolSet:
  - "[[C 指针]]"
knowledgeLink:
  - "[[Dynamic Programming 动态规划]]"
status: true
---
### 管线渲染的基本过程

1. 初始化三角网格
    1. **顶点**位置
    2. **顶点**属性
2. 初始化摄像机
3. 移动三角网格与摄像机
4. 以**摄像机**为中心观察世界
5. 投影世界
    1. 删除被遮罩的物体
6. 顶点投射至屏幕
7. 屏幕顶点着色

### 渲染管线的2大阶段7大步骤

**第一阶段**

1. 顶点数据
    1. **位置**：顶点坐标
    2. **颜色**：顶点rgb颜色
    3. **法线**、uv、切线
2. 三维变换
    1. **模型变换**：**矩阵**操作**将三角顶点**平移旋转缩放
    2. **视图变换**：**矩阵**操作**将三角顶点**变换到**摄像机为中心**的坐标系中
    3. **投影变换**：**矩阵**操作**将三角顶点**变换到**屏幕坐标系（投影平面）** 中
3. 图元装配
    1. **图元Primitive**：点和线构成的**几何关系**称为图元
    2. **图元的构成**： **三角形（三个顶点）**、**直线（两个顶点）**，是变换完毕的点进行构成。**顶点+构成顺序**便是图元（0-1-2和0-2-1是两个完全不同的图元，因为顶点相同顺序不同）
    3. **图元的装配**：把**变换后**的**顶点**按照**顺序**组成具体的形状的过程叫做**装配**
4. 剪裁剔除
    1. **剪裁**：视口外无法显示的图元都裁掉，增加渲染效率
    2. **剔除**：**不**渲染三角形的**背面**（非法线朝向，法线朝向与图元装配**顺序**有关）

**第二阶段**

1. 光栅化
    1. 屏幕由一格格的像素组成，虚拟的几何图形也要做成栅格才能渲染，这个过程就是**光栅化**
    2. 此阶段并**不决定**像素具体颜色，仅决定顶点所包围的**区域范围**
2. 片元着色
    1. 计算每个像素**片元**最终显示的颜色（即光栅化阶段的范围颜色）
3. 混合与测试
    1. **混合**：决定**半透明**的效果
    2. **测试**：决定图层的先后顺序