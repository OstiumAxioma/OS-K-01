---
date: 2024-01-17
tags:
  - 计算机/数据结构
  - 计算机/编程语言
  - 计算机/数据结构/抽象类型
  - "#计算机/复杂度/时间复杂度"
component:
  - "[[C 指针]]"
timeComplexity:
  - O(1)
spaceComplexity:
  - O(n^2)
status: false
---
## 数据结构简介
图（graph）是一种非线性数据结构，由顶点（vertex）和边（edge）组成。我们可以将图 $G$ 抽象地表示为一组顶点$E$ 
 和一组边 $V$ 的集合。以下示例展示了一个包含 5 个顶点和 7 条边的图。
 $$
 \begin{aligned}
&\text{V} =\{1,2,3,4,5\}  \\
&\text{E} =\{(1,2),(1,3),(1,5),(2,3),(2,4),(2,5),(4,5)\}  \\
&\text{G} =\{V,E\} 
\end{aligned}
$$
如果将顶点看作节点，将边看作连接各个节点的引用（指针），我们就可以将图看作一种从链表拓展而来的数据结构。如图 9-1 所示，相较于线性关系（链表）和分治关系（树），网络关系（图）的自由度更高，因而更为复杂
### 数据结构历史
一般认为，欧拉于1736年出版的关于柯尼斯堡七桥问题的论文是图论领域的第一篇文章。此问题被推广为著名的欧拉路问题，亦即一笔画问题。而此论文与范德蒙德的一篇关于骑士周游问题的文章，则是继承了莱布尼茨提出的“位置分析”的方法。欧拉提出的关于凸多边形顶点数、棱数及面数之间的关系的欧拉公式与图论有密切联系，此后又被柯西等人进一步研究推广，成了拓扑学的起源。1857年，哈密顿发明了“环游世界游戏”（icosian game），与此相关的则是另一个广为人知的图论问题“哈密顿路径问题”。

西尔维斯特于1878年发表在《自然》上的一篇论文中首次提出“图”这一名词。

欧拉的论文发表后一个多世纪，凯莱研究了在微分学中出现的一种数学分析的特殊形式，而这最终将他引向对一种特殊的被称为“树”的图的研究。由于有机化学中有许多树状结构的分子，这些研究对于理论化学有着重要意义，尤其是其中关于具有某一特定性质的图的计数问题。除凯莱的成果外，波利亚也于1935至1937年发表了一些成果，1959年，De Bruijn做了一些推广。这些研究成果奠定了图的计数理论的基础。凯莱将他关于树的研究成果与当时有关化合物的研究联系起来，而图论中有一部分术语正是来源于这种将数学与化学相联系的做法。

四色问题可谓是图论研究史上最著名也是产生成果最多的问题之一：“是否任何一幅画在平面上的地图都可以用四种颜色染色，使得任意两个相邻的区域不同色？”这一问题由法兰西斯·古德里于1852年提出，而最早的文字记载则出现在德摩根于1852年写给哈密顿的一封信上。包括凯莱、肯普等在内的许多人都曾给出过错误的证明。泰特（Peter Guthrie Tait）、希伍德、拉姆齐和Hadwige（Hugo Hadwiger）对此问题的研究与推广引发了对嵌入具有不同亏格的曲面的图的着色问题的研究。一百多年后，四色问题仍未解决。1969年，Heinrich Heesch发表了一个用计算机解决此问题的方法。1976年，凯尼斯·阿佩尔和沃夫冈·哈肯借助计算机给出了一个证明，此方法按某些性质将所有地图分为1936类并利用计算机一一验证了它们可以用四种颜色染色。但此方法由于过于复杂，在当时未被广泛接受。

1860年之1930年间，若当、库拉托夫斯基和惠特尼从之前独立于图论发展的拓扑学中吸取大量内容进入图论，而现代代数方法的使用更让图论与拓扑走上共同发展的道路。其中应用代数较早者如物理学家基尔霍夫于1845年发表的基尔霍夫电路定律。
### 解决的问题
图与图论在计算机科学领域中有重要的作用。图可以将离散储存在内存不同位置，通过[[C 指针|引用]]互相关联的数据抽象成图论可以分析的内容，并通过图可以进行的不同遍历模式对数据进行增删查。
### 解决思路
如果将顶点看作节点，将边看作连接各个节点的引用（指针），我们就可以将图看作一种从链表拓展而来的数据结构。如图所示，相较于线性关系（[[Link List 链表|链表]]）和分治关系（[[Binary Tree 二叉树|树]]），网络关系（图）的自由度更高，因而更为复杂:

![[Pasted image 20240512155837.png|center|500]]
## 图的常见类型与术语
### 图的组成

图数据结构包含以下常用术语。

<mark class="hltr-orange">邻接（adjacency）</mark>：当两顶点之间存在边相连时，称这两顶点“邻接”。在图中，顶点 1 的邻接顶点为顶点 2、3、5。

<mark class="hltr-cyan">路径（path）</mark>：从顶点 A 到顶点 B 经过的边构成的序列被称为从 A 到 B 的“路径”。在图中，边序列 1-5-2-4 是顶点 1 到顶点 4 的一条路径。

<mark class="hltr-purple">度（degree）</mark>：一个顶点拥有的边数。对于有向图，入度（in-degree）表示有多少条边指向该顶点，出度（out-degree）表示有多少条边从该顶点指出。

![[Pasted image 20240512160503.png|center|]]
### 图的方向
根据边是否具有方向，可分为无向图（undirected graph）和有向图（directed graph），如图所示。

![[Pasted image 20240512160103.png|center|500]]

在<font color="#c0504d">无向图</font>中，边表示两顶点之间的“双向”连接关系，例如微信或 QQ 中的“好友关系”。
在<font color="#4f81bd">有向图</font>中，边具有方向性，即 $A\leftarrow B$ 和 $A\rightarrow B$ 两个方向的边是相互独立的，例如微博或抖音上的“关注”与“被关注”关系。
### 图的连通
根据所有顶点是否连通，可分为连通图（connected graph）和非连通图（disconnected graph），如图所示。

![[Pasted image 20240512160156.png|center|500]]

对于<font color="#0070c0">连通图</font>，从某个顶点出发，可以到达其余任意顶点。
对于<font color="#c0504d">非连通图</font>，从某个顶点出发，至少有一个顶点无法到达。
### 图的权重
我们还可以为边添加“权重”变量，从而得到如图所示的有权图（weighted graph）。例如在《王者荣耀》等手游中，系统会根据共同游戏时间来计算玩家之间的“亲密度”，这种亲密度网络就可以用有权图来表示：

![[Pasted image 20240512160254.png|center|500]]
## 数据结构的实现

在计算机中，图是由“邻接矩阵”和“邻接表”这两种方法来实现的
### 邻接矩阵

设图的顶点数量为 $n$ ，邻接矩阵（adjacency matrix）使用一个 $n\times n$ 大小的矩阵来表示图，每一行（列）代表一个顶点，矩阵元素代表边，用 $1$ 或 $0$ 表示两个顶点之间是否存在边。

如图 9-5 所示，设邻接矩阵为 $M$、顶点列表为 $V$ ，那么矩阵元素 $M[i,j]=1$ 表示顶点 $V[i]$ 到顶点 $V[j]$ 之间存在边，反之 $M[i,j]=0$  表示两顶点之间无边。

![[Pasted image 20240512160755.png|center|500]]

邻接矩阵具有以下特性。

- 顶点不能与自身相连，因此邻接矩阵主对角线元素没有意义。
- 对于无向图，两个方向的边等价，此时邻接矩阵关于主对角线对称。
- 将邻接矩阵的元素从 $1$ 和 $0$ 替换为权重，则可表示[[Graph 图#图的权重|有权图]]。

使用邻接矩阵表示图时，我们可以直接访问矩阵元素以获取边，因此增删查改操作的效率很高，时间复杂度均为$O(1)$ , 然而，矩阵的空间复杂度为 $O(n^2)$ ，内存占用较多

#### 初始化/储存
传入 $n$ 个顶点，初始化长度为 $n$ 的顶点列表 `vertices` ，使用 $O(n)$ 时间；初始化 $n \times n$ 大小的邻接矩阵 `adjMat` ，使用 $O(n^2)$ 时间

```c
/* 基于邻接矩阵实现的无向图结构体 */
typedef struct {
    int vertices[MAX_SIZE];
    int adjMat[MAX_SIZE][MAX_SIZE];
    int size;
} GraphAdjMat;

/* 构造函数 */
GraphAdjMat *newGraphAdjMat() {
    GraphAdjMat *graph = (GraphAdjMat *)malloc(sizeof(GraphAdjMat));
    graph->size = 0;
    for (int i = 0; i < MAX_SIZE; i++) {
        for (int j = 0; j < MAX_SIZE; j++) {
            graph->adjMat[i][j] = 0;
        }
    }
    return graph;
}
```

![[Pasted image 20240512163526.png|center|500]]
<center><sup>图的邻接矩阵初始化</sup></center>
#### 增加

### 邻接表

邻接表（adjacency list）使用 $n$ 个链表来表示图，链表节点表示顶点。第 $i$ 个链表对应顶点 $i$ ，其中存储了该顶点的所有邻接顶点（与该顶点相连的顶点）。下图展示了一个使用邻接表存储的图的示例

![[Pasted image 20240512181150.png|center|500]]
<center><sup>图的邻接表初始化</sup></center>

> [!important]
> 邻接表仅存储实际存在的边，而边的总数通常远小于 $n^2$ ，因此它更加节省空间。然而，在邻接表中需要通过遍历链表来查找边，因此其时间效率不如邻接矩阵

观察上图，邻接表结构与哈希表中的“链式地址”非常相似，因此我们也可以采用类似的方法来优化效率。比如当链表较长时，可以将链表转化为 AVL 树或红黑树，从而将时间效率从 $O(n)$ 优化至 $O(log_n)$；还可以把链表转换为哈希表，从而将时间复杂度降至 $O(1)$
#### 初始化/储存
在邻接表中创建 $n$ 个顶点和 $2m$条边，使用$O(n+m)$ 时间

![[Pasted image 20240512181916.png|center|500]]
<center><sup>邻接表的初始化、增删边、增删顶点</sup></center>
以下是邻接表的代码实现。对比上图，实际代码有以下不同。

1. 为了方便添加与删除顶点，以及简化代码，我们使用列表（动态数组）来代替链表。
2. 使用哈希表来存储邻接表，`key` 为顶点实例，`value` 为该顶点的邻接顶点列表（链表）。

另外，我们在邻接表中使用 `Vertex` 类来表示顶点，这样做的原因是：如果与邻接矩阵一样，用列表索引来区分不同顶点，那么假设要删除索引为 $i$ 的顶点，则需遍历整个邻接表，将所有大于 $i$ 的索引全部减 $1$ ，效率很低。而如果每个顶点都是唯一的 `Vertex` 实例，删除某一顶点之后就无须改动其他顶点了。

```c
/* 节点结构体 */
typedef struct AdjListNode {
    Vertex *vertex;           // 顶点
    struct AdjListNode *next; // 后继节点
} AdjListNode;

/* 查找顶点对应的节点 */
AdjListNode *findNode(GraphAdjList *graph, Vertex *vet) {
    for (int i = 0; i < graph->size; i++) {
        if (graph->heads[i]->vertex == vet) {
            return graph->heads[i];
        }
    }
    return NULL;
}
```

#### 增加

```

```

#### 删除

```

```

#### 访问

```

```

#### 查找

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