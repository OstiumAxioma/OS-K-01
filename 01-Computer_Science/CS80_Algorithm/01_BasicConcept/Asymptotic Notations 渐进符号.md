---
Date: 
tags:
  - 计算机/数据结构
  - 计算机/编程语言
  - "#计算机/复杂度/时间复杂度"
Component: 
Stategy:
  - "[[Algorithm Basic 算法基础]]"
Status:
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

> [!Definition] 𝑂-Notation (Big-Oh)
> - We say (define):
> 	- A function 𝑓 𝑛 is in big-Oh of 𝑔(𝑛) (denoted 𝑂(𝑔 (𝑛)) **iff** (if and only if) there exist positive constants 𝑐 and 𝑛! such that 0 ≤ 𝑓 𝑛 ≤ 𝑐𝑔(𝑛) for all 𝑛 that’s greater than or equal to 𝑛! .
> - In other words,
> 	- 𝑂 (𝑔 (𝑛)) is a set of all functions (call any such function 𝑥 (𝑛) , to avoid confusion with 𝑓(𝑛) ) where there exist positive constants 𝑐 and $𝑛_0$ such that 0 ≤ 𝑥 (𝑛) ≤ 𝑐𝑔(𝑛) for all 𝑛 that’s greater than or equal to $𝑛_0$ .
> 	- And 𝑓(𝑛) is a member of the set 𝑂(𝑔 𝑛 ) (is in the set).
> 	- Proper notation would be: 𝑓(𝑛) ∈ 𝑂(𝑔 𝑛 ), but we abuse ∈ , and write 𝑓 (𝑛) = 𝑂(𝑔 (𝑛) ) most of the time.
> - What does all this mean?
> 	- Study the worked examples in the following slides

