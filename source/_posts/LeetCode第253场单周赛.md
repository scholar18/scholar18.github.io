---
title: LeetCode第253场单周赛
author: 曹毅
date: 2021-08-08-16:54:00
---
# 每张一画

# LeetCode第253场单周赛
就做出来一题，排名2755 / 4569，越来越不行了，不，一直都不行。

## A.检查字符串是否为数组前缀
给你一个字符串 s 和一个字符串数组 words ，请你判断 s 是否为 words 的 前缀字符串 。

字符串 s 要成为 words 的 前缀字符串 ，需要满足：s 可以由 words 中的前 k（k 为 正数 ）个字符串按顺序相连得到，且 k 不超过 words.length 。

如果 s 是 words 的 前缀字符串 ，返回 true ；否则，返回 false 。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/check-if-string-is-a-prefix-of-array
## 思路
这题太简单了，没什么好说的。

挨个遍历即可。
## AC代码
```cpp
class Solution {
public:
    bool isPrefixString(string s, vector<string>& words) {
        string s1;
        int n = words.size();
        for(int i = 0;i < n;i++){
            s1 += words[i];
            if(s == s1) return true;
        }
        return false;
    }
};
```

## B.移除石子使总数最小
给你一个整数数组 piles ，数组 下标从 0 开始 ，其中 piles[i] 表示第 i 堆石子中的石子数量。另给你一个整数 k ，请你执行下述操作 恰好 k 次：

选出任一石子堆 piles[i] ，并从中 移除 floor(piles[i] / 2) 颗石子。
注意：你可以对 同一堆 石子多次执行此操作。

返回执行 k 次操作后，剩下石子的 最小 总数。

floor(x) 为 小于 或 等于 x 的 最大 整数。（即，对 x 向下取整）。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/remove-stones-to-minimize-the-total

### 思路
**贪心+优先队列**
这题忙活半天，我优先队列用错了，或者说我想用优先队列，但我实际上用的是队列。
还是自己没学明白。
之后又复习了优先队列的内容，这里就不具体描述了，我会单开一章。
贪心：我们想当然的希望每次移除的石头堆的石头数量为全堆里最多的，优先队列就是很好的实现方法。


进行k次循环，每次移除一处的石头
将石头存入队列中，每次去除顶部堆，减去移除的石头数再存入。

最后将他们加起来，即可得到答案。

###AC代码
```cpp
class Solution {
public:
    int minStoneSum(vector<int>& p, int k) {
        priority_queue<int> q;
        int n = p.size(),ans = 0;
        for(int i = 0;i < n;i++) q.push(p[i]);
        while(k--){
            int temp = q.top();
            q.pop();
            temp -= temp / 2;
            q.push(temp);
        }
        while(!q.empty()){
            ans += q.top();
            q.pop();
        }
        return ans;
    }
};
```

## C.使字符串平衡的最小交换次数
给你一个字符串 s ，下标从 0 开始 ，且长度为偶数 n 。字符串 恰好 由 n / 2 个开括号 '[' 和 n / 2 个闭括号 ']' 组成。

只有能满足下述所有条件的字符串才能称为 平衡字符串 ：

字符串是一个空字符串，或者
字符串可以记作 AB ，其中 A 和 B 都是 平衡字符串 ，或者
字符串可以写成 [C] ，其中 C 是一个 平衡字符串 。
你可以交换 任意 两个下标所对应的括号 任意 次数。

返回使 s 变成 平衡字符串 所需要的 最小 交换次数。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/minimum-number-of-swaps-to-make-the-string-balanced

### 思路
所谓平衡字符串就是【 在前，】在后，每一个先出现的【，后面都有一个】可以与之匹配，这题我们就是想找出明明应该后出现的】，却先出现的】，然后将前一半改为【，即可。
### AC代码
```cpp
class Solution {
public:
    int minSwaps(string s) {
        int count = 0,res = 0;
        for(auto i:s){
            if(i == '[') count++;
            else{
                if(count > 0){
                    count--;
                }else{
                    res++;
                }
            }
        }
        return (res+1) / 2;
    }
};
```
## D.找出到每个位置为止最长的有效障碍赛跑路线
你打算构建一些障碍赛跑路线。给你一个 下标从 0 开始 的整数数组 obstacles ，数组长度为 n ，其中 obstacles[i] 表示第 i 个障碍的高度。

对于每个介于 0 和 n - 1 之间（包含 0 和 n - 1）的下标  i ，在满足下述条件的前提下，请你找出 obstacles 能构成的最长障碍路线的长度：

你可以选择下标介于 0 到 i 之间（包含 0 和 i）的任意个障碍。
在这条路线中，必须包含第 i 个障碍。
你必须按障碍在 obstacles 中的 出现顺序 布置这些障碍。
除第一个障碍外，路线中每个障碍的高度都必须和前一个障碍 相同 或者 更高 。
返回长度为 n 的答案数组 ans ，其中 ans[i] 是上面所述的下标 i 对应的最长障碍赛跑路线的长度。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/find-the-longest-valid-obstacle-course-at-each-position

### 思路
思路就是没有思路，笑死，我题都看不懂
