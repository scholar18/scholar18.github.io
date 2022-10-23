---
title: 朴素Dijkstra
date: 2021-11-25 10:17:00
author: scholar
# img: ../images/哈希.jpg
hide: false
summary: 图论
---
# 朴素Dijkstra学习笔记
## 适用范围
- 最短路径问题->单源最短路->所有边权都是正数
- **稠密图**，即边数m与$n^2$相近
- 用**邻接矩阵**储存
## 时间复杂度

$$O(N^2)$$  
**N为点数**
## 思想解释
- **dist**表示点离起点的距离，s存储每个点离起点的**最短值**
- 1.dist[1] = 0; // 表示1到起点距离为0
- 2.遍历所有点，找到**不属于s的点**中离起点最近的点，记录为**t**
- 将其存入**s**中
- 用**t**这一点更新**其他点**离起点的距离

## 例题
https://www.acwing.com/problem/content/851/

## AC代码
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 510;

int dist[N];
bool s[N];
int g[N][N];
int n, m;

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    for (int i = 0; i < n; i ++)
    {
        int t = -1;
        for (int j = 1; j <= n; j++)
            if(!s[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
        s[t] = true;
        
        for (int j = 1; j <= n; j ++)
            dist[j] = min(dist[j], dist[t] + g[t][j]);
    }
    if(dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
int main()
{
    memset(g, 0x3f, sizeof g);
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= m; i ++) 
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = min(g[a][b], c);
    }
    printf("%d",dijkstra());
    
    return 0;
}
```