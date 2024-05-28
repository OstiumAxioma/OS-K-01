---
date: 2024-05-09
tags:
  - 计算机/数据结构
  - 计算机/编程语言
  - "#计算机/复杂度/时间复杂度"
component: 
stategy:
  - "[[Algorithm Basic 算法基础]]"
status: true
---
## 什么是渐进符号
大O符号（英语：Big O notation），又称为渐进符号，是用于描述函数渐近行为的数学符号。更确切地说，它是用另一个（通常更简单的）函数来描述一个函数数量级的渐近上界。在数学中，它一般用来刻画被截断的无穷级数尤其是渐近级数的剩余项；在计算机科学中，它在分析算法复杂性的方面非常有用。

大O符号是由德国数论学家保罗·巴赫曼在其1892年的著作《解析数论》（Analytische Zahlentheorie）首先引入的。而这个记号则是在另一位德国数论学家爱德蒙·兰道的著作中才推广的，因此它有时又称为兰道符号（Landau symbols）。代表“order of ...”（……阶）的大O，最初是一个大写希腊字母“Ο”（omicron），现今用的是大写拉丁字母“O”。

> [!question] Why we use big O
> - A mathematical tool (framework) used for describing algorithm’s efficiency, considering the two abstractions we saw:
> 	- Constant factors are not important.
> 	- The most dominating term matters (growth rate)
> - Fairly theoretical, mostly for understanding/gaining insights in analysis of algorithms
> 	- Formal definitions (big-Oh, big-Omega, ...)
> 	- Note, in real world, constant factors and non-dominating terms could  matter (sometimes seriously)

> [!Definition] 𝑂-Notation (Big-O)
> - We say (define):
> 	- A function 𝑓 𝑛 is in big-Oh of 𝑔(𝑛) (denoted 𝑂(𝑔 (𝑛)) **iff** (if and only if) there exist positive constants 𝑐 and $n_0$  such that 0 ≤ 𝑓 𝑛 ≤ 𝑐𝑔($n_0$ ) for all 𝑛 that’s greater than or equal to $n_0$ .

 - In other words,
	- 𝑂 (𝑔 (𝑛)) is a set of all functions (call any such function 𝑥 (𝑛) , to avoid confusion with 𝑓(𝑛) ) where there exist positive constants 𝑐 and $𝑛_0$ such that 0 ≤ 𝑥 (𝑛) ≤ 𝑐𝑔(𝑛) for all 𝑛 that’s greater than or equal to $𝑛_0$ .
	- And 𝑓(𝑛) is a member of the set 𝑂(𝑔 𝑛 ) (is in the set).
	- Proper notation would be: 𝑓(𝑛) ∈ 𝑂(𝑔 𝑛 ), but we abuse ∈ , and write 𝑓 (𝑛) = 𝑂(𝑔 (𝑛) ) most of the time.

## Proof Big-O

> [!question] Sample Question
> Is $\frac12n^2-3n=O(n^2)$ ?

According to the Big-O definition, to proof this, we need to find $c$ and $n_0$ such that:
$$
0\leq\frac12n^2-3n\leq cn^2
$$
For this example, we may find that:
- As long as $c$ is at least $1/2$, the inequality holds for any non-negative $n$.
- Thus, we can let $c=\frac{1}{2}$, $n_0=6$, to satisfy the definition.

> [!important] Range of Acceptation
> In fact, there are infinite amount of  $c$ and $n_0$ for this proof.

These  $c$ and $n_0$ will give us a graph, where when $n>n_0$ , the asymptotic statement will always hold.

![[Pasted image 20240513053814.png|center]]
In the case above, we can conclude that:

> [!success] Nature of Asymptotic
> Rate of growth of a higher order term can’t be overcome by a constant factor 𝒄 multiplied to a lower order term (𝒏), no matter how big 𝒄 can get.

We may also find that lower order term can be always ignored by setting $𝑛_0$ to sufficiently large number.

> [!tip] Rule of thumb
> Pick the highest order term only, drop the constant factor, and that's the best big-O notation for a given function
## Bounding Properties of Asymptotic Notations
### Asymptotically up bound 渐近上界记号：O(big-oh)

> [!Definition]
> 设$f(n)$和$g(n)$是定义域为自然数集$N$上的函数。若存在正数$c$和$n_0$，使得对一切$n\geq n_0$都有$0\leq f(n)\leq cg(n)$成立，则称$f(n)$的渐进上界为$g(n)$，记作：

$$
f(n) = O(g(n))
$$
我们也可以说，函数$f(n)$的阶不高于函数$g(n)$。

根据符号$O$的定义，用它评估算法的复杂度得到的只是问题规模充分大时的一个上界。对于任意一种描述算法复杂度的函数$f(n)$都能找到无数符合条件的函数上界$g(n)$。因此这个上界的阶越低，意味着越贴近函数$f(n)$的实际阶，其精度也越有价值。

> [!example] 设$f(n)=n^2+n$
> $f(n)=O(n^2)$，取$c=2,n_0=1$即可
> $f(n)=O(n^3)$，取$c=1,n_0=2$即可
> 两个$c$和$n_0$的取值都符合条件，但$O(n^2)$作为上界更为精准

常见的复杂度关系可以参考[[Time Complexity 时间复杂度#2.3.4 常见类型]]。

纯数学表达为：
$$
\exists c,n_0>0 :\forall n \ge n_0,f(n)\le g(n)\rightarrow f(n)=O[g(n)]
$$
### Asymptotically low bound 渐近下界记号：Ω(big-omega)
大Ω符号的定义与大O符号的定义类似，但主要区别是，大O符号表示函数在增长到一定程度时总小于一个特定函数的常数倍，大Ω符号则表示总大于
 > [!Definition]
 > 设$f(n)$和$g(n)$是定义域为自然数集$N$上的函数。若存在正数$c$和$n_0$，使得对一切$n\geq n_0$都有$0\leq cg(n)\leq f(n)$成立，则称$f(n)$的渐进上界为$g(n)$，记作：

$$
f(n) = \Omega(g(n))
$$
根据符号$\Omega$的定义，用它评估算法的复杂度得到的只是问题规模充分大时的一个下界。对于任意一种描述算法复杂度的函数$f(n)$都能找到无数符合条件的函数下界$g(n)$。因此这个下界的阶越高，意味着越贴近函数$f(n)$的实际阶，其精度也越有价值。

> [!example] 设$f(n)=n^2+n$
> $f(n)=O(n^2)$，取$c=1,n_0=1$即可
> $f(n)=O(100n)$，取$c=1/100,n_0=1$即可
> 两个$c$和$n_0$的取值都符合条件，但$O(n^2)$作为下界更为精准

纯数学表达为：
$$
\exists c,n_0>0 :\forall n \ge n_0,f(n)\ge g(n)\rightarrow f(n)=\Omega[g(n)]
$$
### Asymptotically tight bound 渐近紧确界记号 Θ（big-theta）
大Θ符号表示函数在某个区间上的渐近关系。如果两个函数在某个区间上的上界和下界都分别为另一个函数，那么这两个函数在该区间上是渐近相等的，可以用大Θ符号表示为：
$$f(n) = Θ(g(n))$$
其中，$n$是区间的变量

假设算法A的运行时间表达式$T_1(n)$为：$T_1\left(n\right)=30n^4+20n^3+40n^2+46n+100$
假设算法B的运行时间表达式$T_2(n)$为：$T_2\left(n\right)=1000n^3+50n^2+78n+10$

当问题规模足够大的时候，例如$n=10e^{1000}$，算法的运行时间将主要取决于时间表达式的第一项，其它项的执行时间只有它的几十万分之一，可以忽略不计。第一项的常数系数，随着$n$的增大，对算法的执行时间也变得不重要了。

于是，
算法A的运行时间可以记为：$T_1\left(n\right)\approx n^4,T_1\left(n\right)=\Theta(n^4)$
算法B的运行时间可以记为：$T_2\left(n\right)\approx n^3,T_2\left(n\right)=\Theta(n^3)$

> [!Definition] Θ的数学含义
>  设$f(n)$和$g(n)$是定义域为自然数集$N$上的函数。若$\operatorname*{lim}_{n\to\infty}\frac{f(n)}{g(n)}$存在，并且等于某个常数$c(c>0)$，那么$f(n)=\Theta(g(n))$

我们也可以通俗的理解$f(n)=\Theta(g(n))$意味着$f(n)$和$g(n)$同阶。

> [!NOTE] 寻找$\Theta g(n)$
> 存在常数$c_1$，$c_2$和$n_0$，使得对所有$n\geq n_0$，有$0\leq c_1g(n)\leq f(n)\leq c_2g(n)$，且函数在$n\geq n_0$的区间内夹入$c_1g(n)$与$c_2g(n)$之间，则$f(n)$属于集合$\Theta(g(n))$，记作$f(n)\in\Theta(g(n))$。通常我们看到的$f(n)=\Theta(g(n))$是$f(n)\in\Theta(g(n))$的一种代替，其中的”$=$“**并不完全是等于的意思**

纯数学表达：
$$
\exists c_1,c_2,n_0>0 :\forall n \ge n_0,0\leq c_1g(n)\leq f(n)\leq c_2g(n)\rightarrow f(n)=O[g(n)]
$$

由下图中左侧$f(n)=\Theta(g(n))$的图像可以看出，对所有$n\geq n_0$的区间范围内，函数$f(n)$乘以一个常量因子$c$可得出$g(n)$。这就是渐近紧确界的几何意义。$\Theta$记号在五个记号中，要求是最严格的，因为$g(n)$需要同时满足上界和下界的要求。

![[Pasted image 20240513233049.png|center]]

大Θ符号具有以下性质：

反对称性：如果 $f(n) = Θ(g(n))$，那么 $g(n) = Θ(f(n))$。
传递性：如果 $f(n) = Θ(g(n))$，$g(n) = Θ(h(n))$，那么$f(n) = Θ(h(n))$。
幂等性：如果 $k$ 是常数，那么 $f(n) = Θ(g(n))$ 等价于 $f(n) = Θ(kg(n))$。
## 渐进记号间的关系

|  **记号**  | **含义** | **通俗理解** |
| :------: | :----: | :------: |
| $\Theta$ |  紧确界   |   $=$    |
|   $O$    |   上界   |  $\le$   |
|   $o$    | 非紧的上界  |   &lt;   |
| $\Omega$ |   下界   |  $\ge$   |
| $\omega$ | 非紧的下界  |   &gt;   |