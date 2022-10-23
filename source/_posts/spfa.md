---
title: Spfa
date: 2021-11-25 10:17:00
author: scholar
# img: ../images/哈希.jpg
hide: false
summary: 图论
---
# Spfa算法
## 适用范围
- 最短路->单源最短路->存在负权边
- 不存在负权边也可用
- 判断是否有负权回路
## 时间复杂度
平均$m$, 最坏是$nm$
## 原理
- 与bellman-ford类似，但在第二层循环进行了优化
- 我们只将确认变短的点放进队列内，以此用来更新其他的最短路径
- 大概意思为，有**一点**的**最短路径更新**了，那么以此点为**起点**的**点的最短路径**都要被更新，所以我们用**队列**来存储
- 我们使用**邻接表**来存储图
## 例题
https://www.acwing.com/problem/content/853/

## ACcode
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 100010;

int h[N], e[N], ne[N], w[N], idx;
int dist[N];
bool st[N];
int n, m;

void add(int a, int b, int c)
{
    e[idx] = b, ne[idx] = h[a], w[idx] = c, h[a] = idx ++;
}

void spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    queue<int> q;
    q.push(1);
    st[1] = true;
    while(q.size())
    {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if(dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                if(!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
}

int main()
{
    memset(h, -1, sizeof h);
    cin >> n >> m;
    for (int i = 1; i <= m; i ++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    spfa();
    if(dist[n] == 0x3f3f3f3f) cout << "impossible" << endl;
    else cout << dist[n] << endl;
    return 0;
}
```

## 判断负权回路
- 首先，若有负权回路，那么**一定存在一点**的最短路径是**一直减小**的。
- 那么这一点一定会**多次更新**自己的**最短路径**
- 我们定义一个函数**cnt**用来**计数**该点的**更新次数**
- 显然易得，一个点的**更新次数**是不会**多余图的点数的**
- 当他**大于或等于图**的**总点数**时，存在负权回路

## 例题
https://www.acwing.com/problem/content/854/

## ACcode
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>

using namespace std;

const int M = 10010, N = 2010;

int h[N], e[M], ne[M], w[M], idx;
int dist[N], cnt[N];
bool st[N];
int n, m;

void add(int a, int b, int c)
{
    e[idx] = b, ne[idx] = h[a], w[idx] = c, h[a] = idx ++;
}

bool spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    queue<int> q;
    for (int i = 1; i <= n; i ++)
    {
        st[i] = true;
        q.push(i);
    }
    
    while(q.size())
    {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if(dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if(cnt[j] >= n) return true;
                if(!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    return false;
}

int main()
{
    memset(h, -1, sizeof h);
    cin >> n >> m;
    for (int i = 1; i <= m; i ++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    if(spfa()) puts("Yes");
    else puts("No");
    return 0;
}
```