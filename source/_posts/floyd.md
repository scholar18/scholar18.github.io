---
title: Floyd
date: 2021-11-25 10:17:00
author: scholar
# img: ../images/哈希.jpg
hide: false
summary: 图论
---
# Floyd
## 适用范围
- 多元汇最短路
- 无负权回路
## 时间复杂度
$N^3$, N为点数
## 原理
- 涉及**动态规划**，具体还没搞懂，先记下代码流程
- **三重**循环更新，d[i][j]的**最短路径**
- 每次更新，遍历**所有点**，找到**i到k到j**的**最小值**用来更新
- 同时，存在**负权路径**，所以结果**可能小于INF，但不会小于INF/2**，所以大于INF/2的路径都是**不存在**的
- 切记，floyd算法中需要初始化**d**数组,d[i][j] 表示i到j的距离，初始化为**无穷大**，但**i == j**时，初始化为0.
## 例题
https://www.acwing.com/problem/content/856/
## ACcode
```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 210, INF = 1e9;

int d[N][N];
int n, m, Q;

void floyd()
{
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i ++)
            for (int j = 1; j <= n; j ++)
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
int main()
{
    cin >> n >> m >> Q;
    for (int i = 1; i <= n; i ++)
        for (int j = 1; j <= n; j ++)
            if(i == j) d[i][j] = 0;
            else d[i][j] = INF;
    for (int i = 0; i < m; i ++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        d[a][b] = min(d[a][b], c);
    }
    floyd();
    while(Q --)
    {
        int a, b;
        cin >> a >> b;
        if(d[a][b] > INF / 2) cout << "impossible" << endl;
        else cout << d[a][b] << endl;
    }
    
    return 0;
}
```