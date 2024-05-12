---
date: 2024-04-23
tags:
  - 计算机/算法
  - 计算机
  - 计算机/算法/Leetcode
type:
  - Practice
strategy:
  - "[[Divide and Conquer 分治]]"
  - "[[双指针]]"
timeComplexity: O(n)
spaceComplexity: O(nlogn)
status:
---
# 题目名

## 题目描述

> [!question] Title
> Contents

**示例**：

**输入**：
**输出**：
**解释**：

> [!tip] 
> 

## 解题思路

### 策略1
#### 说明



#### 分析

```c

```

### 策略2

> [!abstract] [[策略]]
> 


> [!important]
> 

> [!info] 最后的答案是什么？
> 答案就是我们每次以双指针为左右边界（也就是「数组」的左右边界）计算出的容量中的最大值

## 代码解

### 代码块

```c
int maxArea(int* height, int heightSize) {
    int left = 0;
    int right = heightSize - 1;
    int max_area = 0;
    while (left < right) {
        int width = right - left;
        int current_area = fmin(height[left], height[right]) * width;
        max_area = fmax(max_area, current_area);
        if (height[left] < height[right]) {
            left++;
        } else {
            right --;
        }
    }
    return max_area;
}
```

### 代码块解说

1. 

## 复杂度分析

- [[Time Complexity 时间复杂度]]：$O(n)$
- [[Space Complexity 空间复杂度]]：$O(1)$

## 其它解

### 暴力解