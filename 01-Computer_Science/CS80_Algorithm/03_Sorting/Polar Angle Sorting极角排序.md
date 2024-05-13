---
date: 2024-05-13
tags:
  - 计算机/数据结构
  - "#计算机/复杂度/时间复杂度"
  - 计算机/算法
  - 计算机/图形学/计算几何
component:
  - "[[Vector向量#Cross Product Calculation|叉乘]]"
stategy:
  - "[[Breadth-First 广度优先]]"
timeComplexity:
  - O(n)
spaceComplexity:
  - O(n)
status: false
---
## 算法简介
极角排序，就是平面上有若干点，选一点作为极点，那么每个点有极坐标 $(p,\theta)$ ，将它们关于极角 $\theta$ 排序。进行极角排序有两种方法：

### 直接计算极角
我们知道极坐标和直角坐标转换公式中有 $\tan\theta=\frac{y}{x}$，所以可以用 $\arctan$ 来计算。然而，$\arctan$ 的值域只有 $(-{\frac{\pi}{2}},{\frac{\pi}{2}})$，而且当 $x=0$
 时无定义，所以需要复杂的分类讨论。所幸，`<cmath>`中有一个`atan2(y,x)`函数，可以直接计算`(x,y)`的极角，值域是$(-\pi,\pi]$，所以可以直接用，只不过需要留心**第四象限的极角会比第一象限小**。

```c
using Points = vector<Point>;
double theta(auto p) { return atan2(p.y, p.x); } // 求极角
void psort(Points &ps, Point c = O)              // 极角排序
{
    sort(ps.begin(), ps.end(), [&](auto p1, auto p2) {
        return lt(theta(p1 - c), theta(p2 - c));
    });
}
```

如果想减少常数，可以提前算出每个点的极角。
### 叉乘算法
第二种方法利用[[Vector向量#Cross Product Calculation|叉乘]]。叉乘的正负遵循右手定则，按旋转方向弯曲右手四指，则若拇指向上叉乘为正，拇指向下叉乘为负。也就是说，如果一个向量通过劣角旋转到另一个向量的方向需要按逆时针方向，那么叉乘为正，否则叉乘为负。

![[Pasted image 20240513014521.png|center]]

仅靠叉乘是不能进行排序的，它不符合偏序关系的条件。因为：
$$
\vec{x}\times\vec{y}<0 \Rightarrow \theta_{\vec{x}}<\theta_{\vec{y}}
$$
我们会发现通过不断在坐标轴上旋转，一个向量的极角最终会小于自己。但是在同一象限内计算叉乘是可以的，所以我们**先比较象限再做叉乘**。

```c
int qua(auto p) { return lt(p.y, 0) << 1 | lt(p.x, 0) ^ lt(p.y, 0); }    // 求象限
void psort(Points &ps, Point c = O)                               // 极角排序
{
    sort(ps.begin(), ps.end(), [&](auto v1, auto v2) {
        return qua(v1 - c) < qua(v2 - c) || qua(v1 - c) == qua(v2 - c) && lt(cross(v1 - c, v2 - c), 0);
    });
}
```

我们用0、1、2、3来表示第一、二、三、四象限。
## 算法复杂度

### 时间复杂度

#### 最坏

#### 最好

### 空间复杂度

## 算法应用

### 实例