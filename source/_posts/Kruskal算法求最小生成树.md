---
title: Kruskal算法求最小生成树
date: 2021-11-30 00:07:00
author: scholar
# img: ../images/哈希.jpg
hide: false
summary: 大数据
---

# Kruskal算法求最小生成树
## 适合范围
- 最小生成树
- 最好稀疏图
## 时间复杂度
$mlogm$
## 过程
- 先以权重进行从小到大的排序
- 遍历每一条边，若该边中两点不在一个集合中，让他俩加入一个集合，统计权重与进入集合个数
- 因为本算法不用考虑边的储存问题,所以我们直接建立一个结构体存贮边，顺便重定义 **<**,方便我们按权重排序
## 例题
https://www.acwing.com/problem/content/861/
## ACcode
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int M = 200010;

int p[M];
int n, m;

struct Edge{
    int a, b, w;
    
    bool operator < (const Edge &W)const 
    {
        return w < W.w;
    }
}edges[M];

int find(int x)
{
    if(p[x] != x) p[x] = find(p[x]);
    return p[x];
}
int main()
{
    cin >> n >> m;
    for (int i = 1; i < n; i ++) p[i] = i;
    for (int i = 0; i < m; i ++)
    {
        int a, b, w;
        cin >> a >> b >> w;
        edges[i] = {a, b, w};
    }
    
    sort(edges, edges + m);
    int res = 0, cnt = 0;
    for (int i = 0; i < m; i++)
    {
        auto t = edges[i];
        int a = find(t.a), b = find(t.b);
        if(a != b)
        {
            p[a] = b;
            res += t.w;
            cnt ++;
        }
    }
    if(cnt < n - 1) cout << "impossible" << endl;
    else cout << res << endl;
    
    return 0;
}
```