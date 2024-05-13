---
date: 2024-01-17
tags:
  - 计算机/数据结构
  - 计算机/数据结构/抽象类型
component:
  - "[[C 数组]]"
  - "[[Array 数组]]"
timeComplexity:
  - O(n)
spaceComplexity:
  - O(n)
status: false
---
## 数据结构简介
队列（queue）是一种遵循先入先出（FIFO, First-In-First-Out）规则的线性数据结构/线性表。顾名思义，队列模拟了排队现象，即新来的人不断加入队列尾部，而位于队列头部的人逐个离开
### 解决的问题
任何需要实现“先来后到”功能的场景，例如打印机的任务队列、餐厅的出餐队列等，队列在这些场景中可以有效地维护处理顺序
### 先进先出
我们将队列头部称为“队首”，尾部称为“队尾”，将把元素加入队尾的操作称为“入队”，删除队首元素的操作称为“出队”
![[Pasted image 20240422232700.png|center|600]]
<center><sup>队列的先入先出规则</sup></center>
## 数据结构的实现

> [!success]
> 为了实现队列，我们需要一种数据结构，可以在一端添加元素，并在另一端删除元素，**链表和数组都符合要求**。
### 基于链表的实现
#### 初始化/储存
我们可以将链表的“头节点”和“尾节点”分别视为“队首”和“队尾”，规定队尾仅可添加节点，队首仅可删除节点。

![[Pasted image 20240422233108.png|center|600]]

```c
/* 基于链表实现的队列 */
typedef struct {
    ListNode *front, *rear;
    int queSize;
} LinkedListQueue;

/* 构造函数 */
LinkedListQueue *newLinkedListQueue() {
    LinkedListQueue *queue = (LinkedListQueue *)malloc(sizeof(LinkedListQueue));
    queue->front = NULL;
    queue->rear = NULL;
    queue->queSize = 0;
    return queue;
}

/* 析构函数 */
void delLinkedListQueue(LinkedListQueue *queue) {
    // 释放所有节点
    while (queue->front != NULL) {
        ListNode *tmp = queue->front;
        queue->front = queue->front->next;
        free(tmp);
    }
    // 释放 queue 结构体
    free(queue);
}

/* 获取队列的长度 */
int size(LinkedListQueue *queue) {
    return queue->queSize;
}

/* 判断队列是否为空 */
bool empty(LinkedListQueue *queue) {
    return (size(queue) == 0);
}
```
#### 增加

```

```

### 删除

```

```

### 访问

```

```

### 查找

```

```

## 数据结构复杂度

### 时间复杂度

- **访问**：$O(1)$，
- **插入**：$O(1)$，
- **删除**：$O(1)$，
- **查找**：$O(N)$，
### 空间复杂度

- **储存**：$O(1)$，

- **访问**：$O(1)$，
- **插入**：$O(1)$，
- **删除**：$O(1)$，
- **查找**：$O(N)$，

## 优势/局限性

## 数据结构应用