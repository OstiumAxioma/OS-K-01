---
date: 2024-05-12
tags:
  - 计算机/算法
  - 计算机/复杂度/时间复杂度
toolSet:
  - "[[Graph 图]]"
knowledgeLink:
  - "[[102-Computational Geometry in a Physics Engine#Bounding Volume]]"
timeComplexity:
  - O(nlogn)
spaceComplexity: 
status: false
---
# Convex Hull Definition
In geometry, the convex hull, convex envelope or convex closure of a shape is the smallest convex set that contains it. The convex hull may be defined either as the intersection of all convex sets containing a given subset of a Euclidean space, or equivalently as the set of all convex combinations of points in the subset. For a bounded subset of the plane, the convex hull may be visualized as the shape enclosed by a rubber band stretched around the subset.

> [!Definition]
> 在计算机图形学中，平面上能包含所有给定点的最小凸多边形都叫做凸包。
> 对于给定集合$X$，所有包含$X$的凸集的交集$S$被称为$S$的凸包。

实际上可以理解为用一个橡皮筋包含住所有给定点的形态。

凸包用最小的周长围住了给定的所有点。如果一个凹多边形围住了所有的点，它的周长一定不是最小，如下图。根据三角不等式，凸多边形在周长上一定是最优的。

![[Pasted image 20240512223727.png|center|300]]

# Convex Hull Algorithm
常用的求法有 Gift Wrapping 算法，Graham 扫描法和 Andrew 算法
## Gift Wrapping 算法


## Andrew 算法
### 算法复杂度
该算法的时间复杂度为 $O(n\log n)$，其中 n 为待求凸包点集的大小，复杂度的瓶颈在于对所有点坐标的双关键字排序。
### 算法实现
首先把所有点以横坐标为第一关键字，纵坐标为第二关键字排序。

显然排序后最小的元素和最大的元素一定在凸包上。而且因为是凸多边形，我们如果从一个点出发逆时针走，轨迹总是「左拐」的，一旦出现右拐，就说明这一段不在凸包上。因此我们可以用一个单调栈来维护上下凸壳。

因为从左向右看，上下凸壳所旋转的方向不同，为了让单调栈起作用，我们首先 升序枚举 求出下凸壳，然后 降序 求出上凸壳。

求凸壳时，一旦发现即将进栈的点$P$ 和和栈顶的两个点（$S_1,S_2$，其中$ S_1$ 为栈顶）行进的方向向右旋转，即[[Vector向量#Cross Product|叉积]]小于 0：  
$\overrightarrow{S_2S_1}\times \overrightarrow{S_1P}<0$，则弹出栈顶，回到上一步，继续检测，直到 $\overrightarrow{S_2S_1}\times \overrightarrow{S_1P}\ge 0$ 或者栈内仅剩一个元素为止。

通常情况下不需要保留位于凸包边上的点，因此上面一段中：
$\overrightarrow{S_2S_1}\times \overrightarrow{S_1P}<0$ 这个条件中的$<$可以视情况改为 $\le$，同时后面一个条件应改为 $>$。

```cpp
// stk[] 是整型，存的是下标
// p[] 存储向量或点
tp = 0;                       // 初始化栈
std::sort(p + 1, p + 1 + n);  // 对点进行排序
stk[++tp] = 1;
// 栈内添加第一个元素，且不更新 used，使得 1 在最后封闭凸包时也对单调栈更新
for (int i = 2; i <= n; ++i) {
  while (tp >= 2  // 下一行 * 操作符被重载为叉积
         && (p[stk[tp]] - p[stk[tp - 1]]) * (p[i] - p[stk[tp]]) <= 0)
    used[stk[tp--]] = 0;
  used[i] = 1;  // used 表示在凸壳上
  stk[++tp] = i;
}
int tmp = tp;  // tmp 表示下凸壳大小
for (int i = n - 1; i > 0; --i)
  if (!used[i]) {
    // ↓求上凸壳时不影响下凸壳
    while (tp > tmp && (p[stk[tp]] - p[stk[tp - 1]]) * (p[i] - p[stk[tp]]) <= 0)
      used[stk[tp--]] = 0;
    used[i] = 1;
    stk[++tp] = i;
  }
for (int i = 1; i <= tp; ++i)  // 复制到新数组中去
  h[i] = p[stk[i]];
int ans = tp - 1;
```

根据上面的代码，最后凸包上有 $\textit{ans}$ 个元素（额外存储了 1 号点，因此 $A$ 数组中有$\textit{ans}+1$ 个元素），并且按<font color="#c0504d">逆时针方向</font>排序。周长就是每一个点$A_i$到下一个点$A_{i+1}$的距离$\left|\overrightarrow{A_iA_{i+1}}\right|$的和：
$$\sum_{i=1}^{\textit{ans}}\left|\overrightarrow{A_iA_{i+1}}\right|$$

## Graham 扫描算法
与 Andrew 算法相同，Graham 扫描法的时间复杂度为 $O(n\log n)$，复杂度瓶颈也在于对所有点排序。