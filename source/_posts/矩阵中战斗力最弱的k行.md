---
title: 矩阵中战斗力最弱的k行
author: 曹毅
date: 2021-08-01-22:40:00
---
# 矩阵中战斗力最弱的k行
给你一个大小为 m * n 的矩阵 mat，矩阵由若干军人和平民组成，分别用 1 和 0 表示。

请你返回矩阵中战斗力最弱的 k 行的索引，按从最弱到最强排序。

如果第 i 行的军人数量少于第 j 行，或者两行军人数量相同但 i 小于 j，那么我们认为第 i 行的战斗力比第 j 行弱。

军人 总是 排在一行中的靠前位置，也就是说 1 总是出现在 0 之前。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/the-k-weakest-rows-in-a-matrix

## 提示
- m == mat.length
- n == mat[i].length
- 2 <= n, m <= 100
- 1 <= k <= m
- matrix[i][j] 不是 0 就是 1

## 示例一
输入：mat =
[
    [1,1,0,0,0],
    [1,1,1,1,0],
    [1,0,0,0,0],
    [1,1,0,0,0],
    [1,1,1,1,1]
], 
k = 3
输出：[2,0,3]
解释：
每行中的军人数目：
行 0 -> 2 
行 1 -> 4 
行 2 -> 1 
行 3 -> 2 
行 4 -> 5 
从最弱到最强对这些行排序后得到 [2,0,3,1,4]
## 示例二
输入：mat = 
[
    [1,0,0,0],
    [1,1,1,1],
    [1,0,0,0],
    [1,0,0,0]
], 
k = 2
输出：[0,2]
解释： 
每行中的军人数目：
行 0 -> 1 
行 1 -> 4 
行 2 -> 1 
行 3 -> 1 
从最弱到最强对这些行排序后得到 [0,2,3,1]

## 思路
这题算是简单题，不过还是有一点小坑
搜集每一行的战斗力大小，然后以从弱到强排序，输出前k行
的行数
但这题要输出的是行数，不是每行的战斗力，这就有点烦
这时，突然想到，好像c++里有个STL库，STL库里有个queue，
哦，这题可以用优先队列，那将每行战斗力与行数相对应，存入队列中，然后把多余的，强的战斗力，pop出去，将剩下的依次存入数组中，最后反转数组，得到答案
那么，我们就可以写代码了

## AC代码
```cpp
class Solution {
public:
    vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
        priority_queue<pair<int, int>> q;
        int n = mat.size();
        for(int i = 0; i < n; i++) {
            int cnt = 0;
            for(int x : mat[i]) {
                cnt += x;
            }
            q.push({cnt, i});
        }
        vector<int> ret;
        while(q.size() > k) q.pop();
        while(!q.empty()) {
            ret.push_back(q.top().second);
            q.pop();
        } 
        reverse(ret.begin(), ret.end());
        return ret;
    }
};
```