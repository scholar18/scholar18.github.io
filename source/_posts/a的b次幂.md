---
title: a的b次幂位运算
date: 2021-10-17 10:17:00
author: scholar
img: ../images/哈希.jpg
hide: false
summary: 利用二进制进行快速幂
---

# a^b,高精度
## 题目
求 a 的 b 次方对 p 取模的值。

### 输入格式
三个整数 a,b,p ,在同一行用空格隔开。

### 输出格式
输出一个整数，表示a^b mod p的值。

### 数据范围
- 0 ≤ a,b ≤ 109
- 1 ≤ p ≤ 109
## 题解
3^9 = 3^(1 + 0 + 0 + 8);
9 = 1001;
即每一位上，**若是1，就乘a**。
每**一步**a * a;
## AC代码
```cpp
#include<iostream>
using namespace std;
int main()
{
    int a,b,p;
    cin >> a >> b >> p;
    int res = 1 % p; // 防止b = 0，p = 1，出现错误
    while(b){
        if(b & 1) res = res * 1ll * a % p;
        // 1ll可以转换为long long 类型
        a = a * 1ll * a % p;
        b >>= 1; // 将b向右移动一位
    }
    cout << res << endl;
}
```