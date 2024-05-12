---
date: 2024-01-17
tags:
  - "#计算机/复杂度/时间复杂度"
  - 计算机/复杂度/空间复杂度
  - 计算机/算法
  - 计算机/算法/策略
component:
  - "[[C 指针]]"
stategy:
  - "[[Greedy Algorithm 贪心算法]]"
timeComplexity:
  - O(n)
spaceComplexity:
  - O(n)
status: false
---
## 算法简介

### 算法历史

### 解决的问题

> [!question] Title
> 给定 𝑛 种硬币，第 𝑖 种硬币的面值为 𝑐𝑜𝑖𝑛𝑠[𝑖−1] ，目标金额为 𝑎𝑚𝑡 ，每种硬币可以重复选取，问能够凑出目标金额的最少硬币数量。如果无法凑出目标金额，则返回 −1 。

### 解决思路

> [!tip] 贪心算法
> 局部解可以导向最优解，采用贪心算法

给定目标金额，**我们贪心地选择不大于且最接近它的硬币**，不断循环该步骤，直至凑出目标金额为止

![[Pasted image 20240421213342.png|center|500]]
## 算法的实现

### 储存/结构

### 策略

### 伪代码

```c

```

### 实现

```c
/* 零钱兑换：贪心 */
int coinChangeGreedy(int *coins, int size, int amt) {
    // 假设 coins 列表有序
    int i = size - 1;
    int count = 0;
    // 循环进行贪心选择，直到无剩余金额
    while (amt > 0) {
        // 找到小于且最接近剩余金额的硬币
        while (i > 0 && coins[i] > amt) {
            i--;
        }
        // 选择 coins[i]
        amt -= coins[i];
        count++;
    }
    // 若未找到可行方案，则返回 -1
    return amt == 0 ? count : -1;
}
```

## 算法复杂度

### 时间复杂度

#### 最坏

#### 最好

### 空间复杂度

## 算法应用

### 实例