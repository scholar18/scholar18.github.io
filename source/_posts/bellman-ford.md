---
title: bellman-ford
date: 2021-11-25 10:17:00
author: scholar
# img: ../images/哈希.jpg
hide: false
summary: 图论
---
# bellman-ford
## 适用范围
- 最短路->单源最短路->存在负权边
- 不存在负权回路
- 存在负权回路，但限制走的路径数

## 时间复杂度
$nm$

**n**为最大路径数, **m**为边数
## 原理
- 最大路径数为**n**, 总边数为**m**
- 两层循环，第一层遍历**n**次，第二层遍历**m**次
- 第二层循环每次更新dist的**最短路径**
- 切记，每次使用**上一次**的结果与**现dist**比较更新最短路
- 因本思路只用记录边数，不用把点连接起来，所以我们直接用**结构体**记录边即可
- 另，因可能存在**负权边**，所以如果**没有**最短路径，dist[n]的答案也可能小于**0x3f3f3f3f**,**最小最小**dist[n]也不会小于**0x3f3f3f3f / 2**,所以我们用这个来判断是否有**最小路径**。
## 例题
https://www.acwing.com/problem/content/855/

## ACcode
```cpp
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 501, M = 10010;

struct Edge
{
    int a, b, w;
}edges[M];

int last[N];
int dist[N];
int n, m, k;

void bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    for (int i = 0; i < k; i ++)
    {
        memcpy(last, dist, sizeof dist);
        for (int j = 1; j <= m; j ++)
        {
            auto x = edges[j];
            dist[x.b] = min(dist[x.b], last[x.a] + x.w);
        }
    }
}
int main()
{
    cin >> n >> m >> k;
    for (int i = 1; i <= m; i ++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        edges[i] = {a, b, c};
    }
    bellman_ford();
    if(dist[n] > 0x3f3f3f3f / 2) cout << "impossible" << endl;
    else cout << dist[n] << endl;
    
    return 0;
}
```