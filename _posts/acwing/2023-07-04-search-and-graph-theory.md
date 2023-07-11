---
title: "03-搜索与图论"
subtitle: "搜索与图论"
layout: post
author: "rainchxy"
header-style: text
hidden: true
tags:
  - 搜索
  - acwing
  - 图论
---

# 基础知识：
- 输入数据规模大于1e6时，才必须用scanf；小于时用cin或scanf都一样。 
- 1e6前面的数字可以是小数，如1.5e6
- memset()头文件，string.h或者cstring

# 概要：
> - 1.DFS:stack O(h)   无最短路性质   回溯，剪枝
> - 2.BFS:queue O(2^h) 有最短路的性质
> - 3.树与图的存储
> - 4.树与图的DFS
> - 5.树与图的BFS
> - 6.拓扑排序

# DFS：
## 
- 思想：
> - stack O(h)   无最短路性质   回溯，剪枝
> - 搜索顺序不同，框架不同；但相同的是先想象成树状结构，再执行以下3个操作：操作现场，DFS()，恢复现场。
- 模板：
- [排列数字](https://www.acwing.com/problem/content/844/)

```
```

# BFS：
## 
- 思想：
> - BFS:queue O(2^h) 有最短路的性质
> - 当所有边权都是1的时候，才能用BFS求最短路
- 模板：
> - queue <- 初始状态； while(queue非空){t=queue.front(); 拓展t;}
> - 用两个数组实现“上下左右”4个方向遍历的方法。
- []()

```
```

# 树与图：
## 树与图的存储：
- 思想：
> - 树是无环连通图
> - 无向图是特殊的有向图(因此只考虑有向图)
> - 邻接矩阵：稠密图 空间复杂度n^2 bool或int g[a][b]
> - 邻接表(数据结构与拉链法的哈系表相似)：最常用 稀疏图 
- 模板：
## 树与图的遍历：
> - 每个点只需遍历一次，因此常需要bool st[N]记录点是否已经遍历。
> - 注意重边与自环
- 树与图的DFS：
> - 思想：
> - 模板：
- 树与图的BFS：
- [树的重心](https://www.acwing.com/problem/content/848/)
- [图中点的层次](https://www.acwing.com/problem/content/849/)

```
//树的存储
// 对于每个点k，开一个单链表，存储k所有可以走到的点。h[k]存储这个单链表的头结点
// h,e,ne,idx共4种对象7种值中：只有h数组的序号、e[]中的值，这2者表示真实的点标号；h[]中的值、e数组的序号、ne[]中的值、ne数组的序号、idx的值，这5者表示的序号与点的序号无关。
int h[N], e[N], ne[N], idx;

// 添加一条边a->b
void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

// 初始化
idx = 0;
memset(h, -1, sizeof h);

//遍历
for (int i = h[t]; i != -1; i = ne[i])
{
    int j = e[i];
}
```

```
//深度优先遍历
int dfs(int u)
{
    st[u] = true; // st[u] 表示点u已经被遍历过

    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j]) dfs(j);
    }
}
```

```
//宽度优先遍历
queue<int> q;
st[1] = true; // 表示1号点已经被遍历过
q.push(1);

while (q.size())
{
    int t = q.front();
    q.pop();

    for (int i = h[t]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true; // 表示点j已经被遍历过
            q.push(j);
        }
    }
}
```


# 图的拓扑序列
- 若一个由图中所有点构成的序列 A 满足：对于图中的每条边 (x,y)，x 在 A 中都出现在 y 之前，则称 A 是该图的一个拓扑序列。
- 有向图才有拓扑序列
- 有向无环图，一定存在一个拓扑序列。（所以有向无环图又叫拓扑图）
- 由"入度，出度"的概念求拓扑序列。(所有入度为0的点，都可以作为拓扑序列的起点;再使用BFS求得。)
- [有向图的拓扑序列](https://www.acwing.com/problem/content/850/)


# Bellman-Ford算法(单源最短路径，有负权值。方法1)
- 模板：
> - 时间复杂度 O(nm), n 表示点数，m 表示边数
> - 可加上备份数组，对模板进行修改
- 使用注意：
> - 不再需要用"邻接矩阵、邻接表"来顺序存储图，用vector+struct存储图即可。

```
int n, m;       // n表示点数，m表示边数
int dist[N];        // dist[x]存储1到x的最短路距离

struct Edge     // 边，a表示出点，b表示入点，w表示边的权重
{
    int a, b, w;
}edges[M];

// 求1到n的最短路距离，如果无法从1走到n，则返回-1。
int bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    // 如果第n次迭代仍然会松弛三角不等式，就说明存在一条长度是n+1的最短路径，由抽屉原理，路径中至少存在两个相同的点，说明图中存在负权回路。
    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0; j < m; j ++ )
        {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            if (dist[b] > dist[a] + w)
                dist[b] = dist[a] + w;
        }
    }

    if (dist[n] > 0x3f3f3f3f / 2) return -1;
    return dist[n];
}
```



References
----------

- <https://www.acwing.com/>
- <https://www.markdownguide.org/>
