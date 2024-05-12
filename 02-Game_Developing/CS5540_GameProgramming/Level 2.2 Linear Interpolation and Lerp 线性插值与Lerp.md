---
date: 2024-05-06
tags:
  - 游戏开发
  - 游戏开发/引擎
toolSet:
  - "[[Level 1.2 Csharp and Unity Graphic]]"
  - "[[Level 2.1 Math and Vector in Unity]]"
knowledgeLink:
---
# Linear Interpolation线性插值
## 何为线性插值
线性插值是数学、计算机图形学等领域广泛使用的一种简单插值方法。

线性插值相比其他插值方式，如抛物线插值，具有简单、方便的特点。 线性插值的几何意义即为概述图中利用过A点和B点的直线来近似表示原函数。 线性插值可以用来近似代替原函数，也可以用来计算得到查表过程中表中没有的数值。

![[Pasted image 20240511141853.png|center|400]]
## 如何进行线性插值
假设我们已知坐标 $(x_0,y_0)$ 与 $(x_1,y_1)$ ，要得到 $[x_0,x_1]$ 区间内某一位置 x 在直线上的值。根据图中所示，我们得到:
$$
\frac{y-y_0}{x-x_0}=\frac{y_1-y_0}{x_1-x_0}
$$
此时，由于 $x$ 值已知，所以可以从公式得到 $y$ 的值:
$$y=y_0+(x-x_0)\frac{y_1-y_0}{x_1-x_0}=y_0+\frac{(x-x_0)y_1-(x-x_0)y_0}{x_1-x_0}$$

![[Pasted image 20240511141306.png|center]]

# Unity Lerp函数

在制作游戏时，有时可以在两个值之间进行线性插值。这是通过 Lerp 函数来完成的。线性插值会在两个给定值之间找到某个百分比的值。例如，我们可以在数字 3 和 5 之间按 50% 进行线性插值以得到数字 4。这是因为 4 是 3 和 5 之间距离的 50%。

在 Unity 中，有多个 Lerp 函数可用于不同类型。对于我们刚才使用的示例，与之等效的将是 `Mathf.Lerp` 函数，如下所示：

```csharp
// 在此示例中，result = 4
float result = Mathf.Lerp (3f, 5f, 0.5f);
```

`Mathf.Lerp` 函数接受 3 个 float 参数：一个 float 参数表示要进行插值的起始值，另一个 float 参数表示要进行插值的结束值，最后一个 float 参数表示要进行插值的**距离**，通常在0-1之间。在此示例中，插值为 0.5，表示 50%。如果为 0，则函数将返回“from”值；如果为 1，则函数将返回“to”值。

> [!success] Lerp的功能1
> 在from和to值中间按0-1划分为数份，并取**插值距离**对应的值，就是Lerp的主要功能

Lerp 函数的其他示例包括 `Color.Lerp` 和 `Vector3.Lerp`。这些函数的工作方式与 `Mathf.Lerp` 完全相同，但是“from”和“to”值分别为 Color 和 Vector3 类型。在每个示例中，第三个参数仍然是一个 float 参数，表示要插值的大小。这些函数的结果是找到一种颜色（两种给定颜色的某种混合）以及一个矢量（占两个给定矢量之间的百分比）。

让我们看看另一个示例：

```csharp
Vector3 from = new Vector3 (1f, 2f, 3f);
Vector3 to = new Vector3 (5f, 6f, 7f);

// 此处 result = (4, 5, 6)
Vector3 result = Vector3.Lerp (from, to, 0.75f);
```

在此示例中，结果为 (4, 5, 6)，因为 4 位于 1 到 5 之间的 75% 处，5 位于 2 到 6 之间的 75% 处，而 6 位于 3 到 7 之间的 75% 处。

> [!success] Lerp的功能2
> 在某些情况下，可使用 Lerp 函数使值随时间平滑

在以下代码中，如果光的强度从 0 开始，则在第一次更新后，其值将设置为 4。下一帧会将其设置为 6，然后设置为 7，再然后设置为 7.5，依此类推。因此，经过几帧后，光强度将趋向于 8，但随着接近目标，其变化速率将减慢。

```csharp
void Update ()
{
    light.intensity = Mathf.Lerp(light.intensity, 8f, 0.5f);
}
```

如果我们不希望与帧率有关，则可以使用以下代码：

```csharp
void Update ()
{
    light.intensity = Mathf.Lerp(light.intensity, 8f, 0.5f * Time.deltaTime);
}
```

这意味着强度变化将按每秒而不是每帧发生