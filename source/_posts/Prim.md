---
title: 朴素Prim
date: 2021-11-27 10:17:00
author: scholar
# img: ../images/哈希.jpg
hide: false
summary: 图论
---
# 朴素Prim
## 适用范围
- 最小生成树,稠密图
- 可带自环,负权路径
## 时间复杂度
$N^2$
## 流程
- 找到一个图的最小生成树，由于**稠密图**，我们用**邻接矩阵**存贮图
- dist[i] -> **0x3f3f3f3f**
- for i in n
- 每次找到**集合外**的离集合距离**最近的点**
- 用这个点**更新**其他点到集合的**距离**
- 把这个点**加入集合**
## 例题
https://www.acwing.com/problem/content/860/
## ACcode
```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 510, INF = 0x3f3f3f3f;

int dist[N];
int g[N][N];
bool st[N];
int n, m;

int prim()
{
    memset(dist, 0x3f, sizeof dist);
    int res = 0;
    for (int i = 0; i < n; i ++)
    {
        int t = -1;
        for (int j  = 1; j <= n; j ++)
            if(!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
        if(i) res += dist[t];
        st[t] = true;
        
        if(i && dist[t] == INF) return INF;
        
        for (int j = 1; j <= n; j ++)
            dist[j] = min(dist[j], g[t][j]);
    }
    
    return res;
}
int main()
{
    memset(g, 0x3f, sizeof g);
    cin >> n >> m;
    for (int i = 1; i <= m; i ++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        // 无向图，所以两头存储
        // 同时有环，只存最小的那个
        g[a][b] = g[b][a] = min(g[a][b], c);
    }
    int t = prim();
    if(t == INF) cout << "impossible" << endl;
    else cout << t << endl;
    return 0;
}
```
