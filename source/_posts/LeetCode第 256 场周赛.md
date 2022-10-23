---
title: LC第256场周赛题解
date: 2021-08-29 09:25:00
author: scholar
img: ../images/哈希.jpg
hide: false
summary: AC两道，排名1925
---

# 解解乏
![本地路径](../images/雷姆.png "相对路径演示,上一级目录")
# LeetCode第256场周赛
这次比较水了，第一题看的时候，很快呀，不到一分钟就提交，然后就错了。
第二题，光想着把string转换为int，还现学了python，忽视了sort函数的强大，string类型也可以排序。
第三题，写半天，用的贪心，但贪心并不能完全满足这题的题意，还需要状态压缩与BFS，没写出来。现在还是比较菜的。
## A.学生分数的最小差值
给你一个 下标从 0 开始 的整数数组 nums ，其中 nums[i] 表示第 i 名学生的分数。另给你一个整数 k 。

从数组中选出任意 k 名学生的分数，使这 k 个分数间 最高分 和 最低分 的 差值 达到 最小化 。

返回可能的 最小差值 。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores

## 思路
简单题，直接排序，之后以k为一个循环遍历就好了
## AC代码
### c++
```cpp
class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
        int min_n = INT_MAX;
        int sum;
        for(int j = 0;j < nums.size() && j+k-1 < nums.size();j++){
            min_n = min(min_n,nums[j+k-1] - nums[j]);
        }
        return min_n;
    }
};
```
### python
```python
class Solution:
    def minimumDifference(self, nums: List[int], k: int) -> int:
        arr = sorted(nums)
        return min(arr[i + k - 1] - arr[i] for i in range(len(nums) - k + 1))
```
- 时间复杂度O(NlogN)
- 空间复杂度O(N)
  
## B.找出数组中的第 K 大整数
给你一个字符串数组 nums 和一个整数 k 。nums 中的每个字符串都表示一个不含前导零的整数。

返回 nums 中表示第 k 大整数的字符串。

注意：重复的数字在统计时会视为不同元素考虑。例如，如果 nums 是 ["1","2","2"]，那么 "2" 是最大的整数，"2" 是第二大的整数，"1" 是第三大的整数。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/find-the-kth-largest-integer-in-the-array

## 思路
简单写一个cmp函数进行sort排序，之后返回第k大的整数
##AC代码
### c++
```cpp
class Solution {
public:
    static bool cmp(string& a,string& b)
    {
        if(a.size() < b.size()) return true;
        else if(a.size() == b.size()) return a < b;
        return false;
    }
    string kthLargestNumber(vector<string>& nums, int k) {
            sort(nums.begin(),nums.end(),cmp);
            int n = nums.size();
            return nums[n-k];
    }
};
```
### python
```Python
class Solution:
    def kthLargestNumber(self, nums: List[str], k: int) -> str:
        return str(sorted(map(int, nums), reverse=True)[k - 1])
```

## C.完成任务的最少工作时间段
你被安排了 n 个任务。任务需要花费的时间用长度为 n 的整数数组 tasks 表示，第 i 个任务需要花费 tasks[i] 小时完成。一个 工作时间段 中，你可以 至多 连续工作 sessionTime 个小时，然后休息一会儿。

你需要按照如下条件完成给定任务：

如果你在某一个时间段开始一个任务，你需要在 同一个 时间段完成它。
完成一个任务后，你可以 立马 开始一个新的任务。
你可以按 任意顺序 完成任务。
给你 tasks 和 sessionTime ，请你按照上述要求，返回完成所有任务所需要的 最少 数目的 工作时间段 。

测试数据保证 sessionTime 大于等于 tasks[i] 中的 最大值 。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks

## 思路
题解的状态压缩没看懂，里面用到了位运算，按位‘与’运算符，我搞不明白，有点晕。
但我看到了另一个题解，随机暴力解法，利用概率随机排序一个很大的数，在我基数很大的情况下，必然会出现一个正确结果。
**这种方法很无耻，所以我就拿来用了**
也就是，随机进行一万，十万，甚至一百万次随机排序，之后按部就班计算每次排序结果所用的工作时间段，取最小值。

## c++
随机化算法
```cpp
class Solution {
public:
    int ck(vector<int>& tasks, int t){
        int cnt = 0, now = 0;
        for(int x: tasks) now + x <= t ? now += x : (now = x, ++cnt);
        return cnt + (now > 0);
    }
    int minSessions(vector<int>& tasks, int sessionTime) {
        int ans = INT_MAX;
        for(int i = 0; i < 2333; ++i){
            random_shuffle(begin(tasks), end(tasks));
            ans = min(ans, ck(tasks, sessionTime));
        }
        return ans;
    }
};
```

这里奉上状态压缩+BFS算法，等我再强点，再补上思路
```cpp
class Solution {
public:
    int minSessions(vector<int>& tasks, int sessionTime) {
        int n = tasks.size(), m = 1 << n;
        constexpr int INF = 20;
        vector<int> dp(m, INF);

        // 预处理每个状态，合法状态预设为 1
        for (int i = 1; i < m; i++) {
            int state = i, idx = 0;
            int spend = 0;
            while (state > 0) {
                int bit = state & 1;
                if (bit == 1) {
                    spend += tasks[idx];
                }
                state >>= 1;
                idx++;
            }
            if (spend <= sessionTime) {
                dp[i] = 1;
            }
        }

        // 对每个状态枚举子集，跳过已经有最优解的状态
        for (int i = 1; i < m; i++) {
            if (dp[i] == 1) {
                continue;
            }
            for (int j = i; j > 0; j = (j - 1) & i) {
                // i 状态的最优解可能由当前子集 j 与子集 j 的补集得来
                dp[i] = std::min(dp[i], dp[j] + dp[i ^ j]);
            }
            // 暴力枚举二进制子集
            // for (int j = 1; j <= i; j++) {
            //     if ((i | j) == i) {
            //         dp[i] = std::min(dp[i], dp[j] + dp[i ^ j]);
            //     }
            // }
        }

        return dp[m - 1];
    }
};
```
## D.不同的好子序列数目
给你一个二进制字符串 binary 。 binary 的一个 子序列 如果是 非空 的且没有 前导 0 （除非数字是 "0" 本身），那么它就是一个 好 的子序列。

请你找到 binary 不同好子序列 的数目。

比方说，如果 binary = "001" ，那么所有 好 子序列为 ["0", "0", "1"] ，所以 不同 的好子序列为 "0" 和 "1" 。 注意，子序列 "00" ，"01" 和 "001" 不是好的，因为它们有前导 0 。
请你返回 binary 中 不同好子序列 的数目。由于答案可能很大，请将它对 109 + 7 取余 后返回。

一个 子序列 指的是从原数组中删除若干个（可以一个也不删除）元素后，不改变剩余元素顺序得到的序列。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/number-of-unique-good-subsequences

### 思路
动态规划，hard题目，现阶段不考虑，有兴趣自己看  ^ _ ^ 
### AC代码
```cpp
class Solution {
public:
    int numberOfUniqueGoodSubsequences(string s) {
        int n = s.size();
        int dp0 = 0, dp1 = 0, mod = 1e9 + 7, has0 = 0;
        for(int i = n-1; i >= 0; --i) {
            if(s[i] == '0') {
                has0 = 1;
                dp0 = (dp0 + dp1 + 1) % mod;
            } else {
                dp1 = (dp0 + dp1 + 1) % mod;
            }
        }
        return (dp1 + has0) % mod;
    }
};
```