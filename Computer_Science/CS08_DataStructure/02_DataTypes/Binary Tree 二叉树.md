---
Date: 2024-03-17
tags:
  - 计算机/数据结构
  - 计算机/编程语言
  - 计算机/数据结构/抽象类型
Component:
  - "[[C 指针]]"
  - "[[Link List 链表]]"
TimeComplexity:
  - O(n)
SpaceComplexity:
  - O(n)
Status:
---
## 数据结构简介
二叉树（binary tree）是一种非线性数据结构，代表“祖先”与“后代”之间的派生关系，体现了“一分为二”的分治逻辑。与链表类似，二叉树的基本单元是节点，每个节点包含值、左子节点引用和右子节点引用。
### 数据结构历史
关于到底是谁发明了二叉树，已经无从得知了，反正历史十分悠久，之所以二叉树会存在，是为了解决线性搜索耗费时间的问题，但是因为二叉树的极端情况（有可能演变为链表），所以二叉树在生产环境下基本没有使用，但是二叉树衍生除了许多其它的变种，比如 红黑树（Red-Black-Tree）， 红黑树的应用无处不在（操作系统，标准库等等），但是要想理解这些变种，基本的二叉树原理，是必须要知道的
### 基本构成/术语
- 根节点（root node）：位于二叉树顶层的节点，没有父节点。
- 叶节点（leaf node）：没有子节点的节点，其两个指针均指向 `None` 。
- 边（edge）：连接两个节点的线段，即节点引用（指针）。
- 节点所在的层（level）：从顶至底递增，根节点所在层为 1 。
- 节点的度（degree）：节点的子节点的数量。在二叉树中，度的取值范围是 0、1、2 。
- 二叉树的高度（height）：从根节点到最远叶节点所经过的边的数量。
- 节点的深度（depth）：从根节点到该节点所经过的边的数量。
- 节点的高度（height）：从距离该节点最远的叶节点到该节点所经过的边的数量。
![[Pasted image 20240422222600.png|center|700]]
<center><sup>二叉树的常用术语</sup></center>
> [!tip]
> 请注意，我们通常将“高度”和“深度”定义为“经过的边的数量”，但有些题目或教材可能会将其定义为“经过的节点的数量”。在这种情况下，高度和深度都需要加 1 。

## 链表实现二叉树

### 初始化/储存
每个节点都有两个引用（指针），分别指向左子节点（left-child node）和右子节点（right-child node），该节点被称为这两个子节点的父节点（parent node）。当给定一个二叉树的节点时，我们将该节点的左子节点及其以下节点形成的树称为该节点的左子树（left subtree），同理可得右子树（right subtree）。

**在二叉树中，除叶节点外，其他所有节点都包含子节点和非空子树**。如图所示，如果将“节点 2”视为父节点，则其左子节点和右子节点分别是“节点 4”和“节点 5”，左子树是“节点 4 及其以下节点形成的树”，右子树是“节点 5 及其以下节点形成的树”。

```c
/* 二叉树节点结构体 */
typedef struct TreeNode {
    int val;                // 节点值
    int height;             // 节点高度
    struct TreeNode *left;  // 左子节点指针
    struct TreeNode *right; // 右子节点指针
} TreeNode;

/* 构造函数 */
TreeNode *newTreeNode(int val) {
    TreeNode *node;

    node = (TreeNode *)malloc(sizeof(TreeNode));
    node->val = val;
    node->height = 0;
    node->left = NULL;
    node->right = NULL;
    return node;
}
```

![[Pasted image 20240422222412.png|center|800]]
<center><sup>父节点、子节点、子树</sup></center>
与链表类似，首先初始化节点，然后构建引用（指针）

```c
/* 初始化二叉树 */
// 初始化节点
TreeNode *n1 = newTreeNode(1);
TreeNode *n2 = newTreeNode(2);
TreeNode *n3 = newTreeNode(3);
TreeNode *n4 = newTreeNode(4);
TreeNode *n5 = newTreeNode(5);
// 构建节点之间的引用（指针）
n1->left = n2;
n1->right = n3;
n2->left = n4;
n2->right = n5;
```

### 增加/删除
与链表类似，在二叉树中插入与删除节点可以通过修改指针来实现。下图给出了一个示例
![[Pasted image 20240422222850.png|center|600]]
<center><sup>在二叉树中插入与删除节点</sup></center>
```c
/* 插入与删除节点 */
TreeNode *P = newTreeNode(0);
// 在 n1 -> n2 中间插入节点 P
n1->left = P;
P->left = n2;
// 删除节点 P
n1->left = n2;
```

> [!important]
> 需要注意的是，插入节点可能会改变二叉树的原有逻辑结构，而删除节点通常意味着删除该节点及其所有子树。因此，在二叉树中，**插入与删除通常是由一套操作配合完成的**，以实现有实际意义的操作。
### 查找/遍历

从物理结构的角度来看，树是一种基于链表的数据结构，因此其遍历方式是通过指针逐个访问节点。然而，树是一种非线性数据结构，这使得遍历树比遍历链表更加复杂，需要借助搜索算法来实现。

> [!important]
> 由于树是基于链表类结构实现的，查找特定元素需要遍历和对比整个树，除非建立了特殊的树结构，比如[[Binary Search Tree 二叉搜索树]]

二叉树常见的遍历方式包括层序遍历、前序遍历、中序遍历和后序遍历等

#### 层序遍历（广度优先）

> [!definition]
> 层序遍历（level-order traversal）从顶部到底部逐层遍历二叉树，并在每一层按照从左到右的顺序访问节点。

层序遍历本质上属于[[Breadth-First 广度优先]]，也称广度优先搜索，它体现了一种“一圈一圈向外扩展”的逐层遍历方式

![[Pasted image 20240422232203.png|center|600]]
<center><sup>二叉树的层序遍历</sup></center>
广度优先遍历通常借助[[Queue 队列]]来实现。队列遵循“[[Queue 队列#先进先出|先进先出]]”的规则，而广度优先遍历则遵循“逐层推进”的规则，两者背后的思想是一致的。实现代码如下：

```c
/* 层序遍历 */
int *levelOrder(TreeNode *root, int *size) {
    /* 辅助队列 */
    int front, rear;
    int index, *arr;
    TreeNode *node;
    TreeNode **queue;

    /* 辅助队列 */
    queue = (TreeNode **)malloc(sizeof(TreeNode *) * MAX_SIZE);
    // 队列指针
    front = 0, rear = 0;
    // 加入根节点
    queue[rear++] = root;
    // 初始化一个列表，用于保存遍历序列
    /* 辅助数组 */
    arr = (int *)malloc(sizeof(int) * MAX_SIZE);
    // 数组指针
    index = 0;
    while (front < rear) {
        // 队列出队
        node = queue[front++];
        // 保存节点值
        arr[index++] = node->val;
        if (node->left != NULL) {
            // 左子节点入队
            queue[rear++] = node->left;
        }
        if (node->right != NULL) {
            // 右子节点入队
            queue[rear++] = node->right;
        }
    }
    // 更新数组长度的值
    *size = index;
    arr = realloc(arr, sizeof(int) * (*size));

    // 释放辅助数组空间
    free(queue);
    return arr;
}
```

- **[[Time Complexity 时间复杂度|时间复杂度]]为 𝑂(𝑛)** ：所有节点被访问一次，使用 𝑂(𝑛) 时间，其中 𝑛 为节点数量。
- **[[Space Complexity 空间复杂度|空间复杂度]]为 𝑂(𝑛)** ：在最差情况下，即满二叉树时，遍历到最底层前，队列中最多同时存在 (𝑛+1)/2 个节点，占用 𝑂(𝑛) 空间。

#### 前序、中序、后序遍历（深度有限）

前序、中序和后序遍历都属于深度优先遍历（depth-first traversal），也称深度优先搜索（depth-first search, DFS），它体现了一种“先走到尽头，再回溯继续”的遍历方式。相对于[[#层序遍历（广度优先）]]，它们也可以称为顺序遍历

> [!definition]
> **顺序遍历**就像是绕着整棵二叉树的外围“走”一圈，在每个节点都会遇到三个位置，分别对应前序遍历、中序遍历和后序遍历

> [!important]
> 前中后序遍历的命名来自遍历过程中访问父节点的顺序，前序就是先访问父节点，中序就是第二位访问父节点，后序遍历则是最后访问父节点。

![[Pasted image 20240422234137.png|center|600]]
<center><sup>二叉搜索树的前序、中序、后序遍历</sup></center>
顺序遍历通常基于递归实现：

下图展示了前序遍历二叉树的递归过程，其可分为“递”和“归”两个逆向的部分。

1. “递”表示开启新方法，程序在此过程中访问下一个节点。
2. “归”表示函数返回，代表当前节点已经访问完毕。

- **[[Time Complexity 时间复杂度|时间复杂度]]为 𝑂(𝑛)** ：所有节点被访问一次，使用 𝑂(𝑛) 时间。
- **[[Time Complexity 空间复杂度|空间复杂度]]为 𝑂(𝑛)** ：在最差情况下，即树退化为链表时，递归深度达到 𝑛 ，系统占用 𝑂(𝑛) 栈帧空间。
##### 前序遍历Pre-Order

```c
/* 前序遍历 */
void preOrder(TreeNode *root, int *size) {
    if (root == NULL)
        return;
    // 访问优先级：根节点 -> 左子树 -> 右子树
    arr[(*size)++] = root->val;
    preOrder(root->left, size);
    preOrder(root->right, size);
}
```

##### 中序遍历In-Order

```c
/* 中序遍历 */
void inOrder(TreeNode *root, int *size) {
    if (root == NULL)
        return;
    // 访问优先级：左子树 -> 根节点 -> 右子树
    inOrder(root->left, size);
    arr[(*size)++] = root->val;
    inOrder(root->right, size);
}
```

##### 后序遍历Post-Order

```c
/* 后序遍历 */
void postOrder(TreeNode *root, int *size) {
    if (root == NULL)
        return;
    // 访问优先级：左子树 -> 右子树 -> 根节点
    postOrder(root->left, size);
    postOrder(root->right, size);
    arr[(*size)++] = root->val;
}
```
## 数组实现二叉树

在链表表示下，二叉树的存储单元为节点 `TreeNode` ，节点之间通过指针相连接。

那么，我们能否用数组来表示二叉树呢？答案是肯定的。
### 映射公式
先分析一个简单案例。给定一棵完美二叉树，我们将所有节点按照层序遍历的顺序存储在一个数组中，则每个节点都对应唯一的数组索引。

根据层序遍历的特性，我们可以推导出父节点索引与子节点索引之间的“映射公式”：**若某节点的索引为 𝑖 ，则该节点的左子节点索引为 2𝑖+1 ，右子节点索引为 2𝑖+2** 。下图展示了各个节点索引之间的映射关系。

![[Pasted image 20240422235137.png|center|600]]
<center><sup>完美二叉树的数组表示</sup></center>
**映射公式的角色相当于链表中的[[C 指针|节点引用（指针）]]**。给定数组中的任意一个节点，我们都可以通过映射公式来访问它的左右子节点

> [!question] Problem
> 完美二叉树是一个特例，在二叉树的中间层通常存在许多 `None` 。由于层序遍历序列并不包含这些 `None` ，因此我们无法仅凭该序列来推测 `None` 的数量和分布位置。**这意味着存在多种二叉树结构都符合该层序遍历序列**。

如图所示，给定一棵非完美二叉树，上述数组表示方法已经失效

![[Pasted image 20240422235329.png|center|600]]
<center><sup>层序遍历序列对应多种二叉树可能性</sup></center>
为了解决此问题，**我们可以考虑在层序遍历序列中显式地写出所有 `None`** 。如图 7-14 所示，这样处理后，层序遍历序列就可以唯一表示二叉树了。示例代码如下：

```c
/* 二叉树的数组表示 */
// 使用 int 最大值标记空位，因此要求节点值不能为 INT_MAX
int tree[] = {1, 2, 3, 4, INT_MAX, 6, 7, 8, 9, INT_MAX, INT_MAX, 12, INT_MAX, INT_MAX, 15};
```

![[Pasted image 20240422235635.png|center|600]]
<center><sup>任意类型二叉树的数组表示</sup></center>
值得说明的是，**完全二叉树非常适合使用数组来表示**。回顾完全二叉树的定义，`None` 只出现在最底层且靠右的位置，**因此所有 `None` 一定出现在层序遍历序列的末尾**。

这意味着使用数组表示完全二叉树时，可以省略存储所有 `None` ，非常方便。下图给出了一个例子。
![[Pasted image 20240423000150.png|center|600]]
<center><sup>完全二叉树的数组表示</sup></center>
### 初始化/储存

```c
/* 数组表示下的二叉树结构体 */
typedef struct {
    int *tree;
    int size;
} ArrayBinaryTree;

/* 构造函数 */
ArrayBinaryTree *newArrayBinaryTree(int *arr, int arrSize) {
    ArrayBinaryTree *abt = (ArrayBinaryTree *)malloc(sizeof(ArrayBinaryTree));
    abt->tree = malloc(sizeof(int) * arrSize);
    memcpy(abt->tree, arr, sizeof(int) * arrSize);
    abt->size = arrSize;
    return abt;
}

/* 析构函数 */
void delArrayBinaryTree(ArrayBinaryTree *abt) {
    free(abt->tree);
    free(abt);
}

/* 列表容量 */
int size(ArrayBinaryTree *abt) {
    return abt->size;
}
```

### 访问

> [!success]
> 用数组实现的二叉树有一个优势，那就是可以利用下标快速访问某个值而省略遍历的步骤

```c
/* 获取索引为 i 节点的值 */
int val(ArrayBinaryTree *abt, int i) {
    // 若索引越界，则返回 INT_MAX ，代表空位
    if (i < 0 || i >= size(abt))
        return INT_MAX;
    return abt->tree[i];
}
```

### 查找/遍历

> [!important]
> 尽管数组可以直接访问对应下标，但毕竟不是哈希表，如果需要查找特定值还是需要借助遍历。下面是数组二叉树的遍历实现：
#### 顺序遍历

> [!tip]
> 顺序遍历数组二叉树和遍历普通数组的逻辑是类似的

```c
/* 层序遍历 */
int *levelOrder(ArrayBinaryTree *abt, int *returnSize) {
    int *res = (int *)malloc(sizeof(int) * size(abt));
    int index = 0;
    // 直接遍历数组
    for (int i = 0; i < size(abt); i++) {
        if (val(abt, i) != INT_MAX)
            res[index++] = val(abt, i);
    }
    *returnSize = index;
    return res;
}
```
#### 层序遍历

```c
/* 深度优先遍历 */
void dfs(ArrayBinaryTree *abt, int i, char *order, int *res, int *index) {
    // 若为空位，则返回
    if (val(abt, i) == INT_MAX)
        return;
    // 前序遍历
    if (strcmp(order, "pre") == 0)
        res[(*index)++] = val(abt, i);
    dfs(abt, left(i), order, res, index);
    // 中序遍历
    if (strcmp(order, "in") == 0)
        res[(*index)++] = val(abt, i);
    dfs(abt, right(i), order, res, index);
    // 后序遍历
    if (strcmp(order, "post") == 0)
        res[(*index)++] = val(abt, i);
}
```

##### 前序遍历Pre-Order

```c
/* 前序遍历 */
int *preOrder(ArrayBinaryTree *abt, int *returnSize) {
    int *res = (int *)malloc(sizeof(int) * size(abt));
    int index = 0;
    dfs(abt, 0, "pre", res, &index);
    *returnSize = index;
    return res;
}
```

##### 中序遍历In-Order

```c
/* 中序遍历 */
int *inOrder(ArrayBinaryTree *abt, int *returnSize) {
    int *res = (int *)malloc(sizeof(int) * size(abt));
    int index = 0;
    dfs(abt, 0, "in", res, &index);
    *returnSize = index;
    return res;
}
```

##### 后序遍历Post-Order

```c
/* 后序遍历 */
int *postOrder(ArrayBinaryTree *abt, int *returnSize) {
    int *res = (int *)malloc(sizeof(int) * size(abt));
    int index = 0;
    dfs(abt, 0, "post", res, &index);
    *returnSize = index;
    return res;
}
```

### 优点/局限

二叉树的数组表示主要有以下优点。

- 数组存储在连续的内存空间中，对缓存友好，访问与遍历速度较快。
- 不需要存储指针，比较节省空间。
- 允许随机访问节点。

然而，数组表示也存在一些局限性。

- 数组存储需要连续内存空间，因此不适合存储数据量过大的树。
- 增删节点需要通过数组插入与删除操作实现，效率较低。
- 当二叉树中存在大量 `None` 时，数组中包含的节点数据比重较低，空间利用率较低。
## 二叉树类型
### 完美二叉树Perfect Binary Tree

> [!definition]
> 完美二叉树（perfect binary tree）所有层的节点都被完全填满

在完美二叉树中，叶节点的度为 0 ，其余所有节点的度都为 2 ；若树的高度为 ℎ ，则节点总数为 $2ℎ+1−1$ ，呈现标准的<font color="#ff5e56">指数级</font>关系，反映了自然界中常见的细胞分裂现象。

![[Pasted image 20240422230127.png|center|600]]
 <center><sup>完美二叉树</sup></center>
### 完全二叉树Complete Binary Tree

> [!definition]
> 完全二叉树（complete binary tree）只有最底层的节点未被填满，且最底层节点尽量靠**左填充**

![[Pasted image 20240422230230.png|center|600]]
<center><sup>完全二叉树</sup></center>
###  完满二叉树Full Binary Tree

> [!definition]
> 完满二叉树（full binary tree）除了叶节点之外，其余所有节点都有两个子节点

![[Pasted image 20240422230613.png|center|600]]
<center><sup>完满二叉树</sup></center>
### 平衡二叉树Balanced Binary Tree

> [!definition]
> 平衡二叉树（balanced binary tree）中任意节点的左子树和右子树的**高度之差的绝对值不超过 1**

![[Pasted image 20240422230714.png|center|600]]
<center><sup>平衡二叉树</sup></center>
## 二叉树的退化
下图展示了二叉树的理想结构与退化结构。当二叉树的每层节点都被填满时，达到“完美二叉树”；而当所有节点都偏向一侧时，二叉树退化为“链表”。

- 完美二叉树是理想情况，可以充分发挥二叉树“分治”的优势。
- 链表则是另一个极端，各项操作都变为线性操作，时间复杂度退化至 $𝑂(𝑛)$ 。

|                | 完美二叉树         | 链表   |
| :------------- | :------------ | :--- |
| 第 𝑖 层的节点数量    | 2𝑖−1         | 1    |
| 高度为 ℎ 的树的叶节点数量 | 2ℎ            | 1    |
| 高度为 ℎ 的树的节点总数  | 2ℎ+1−1        | ℎ+1  |
| 节点总数为 𝑛 的树的高度 | log2⁡(𝑛+1)−1 | 𝑛−1 |

## 数据结构应用