---
title: LeetCode第251次单周赛
date: 2021-07-25 23:15:30
author: 曹毅
# img: /source/images/周赛.jpg
---
# A.字符串转化后的各位数字之和
给你一个由小写字母组成的字符串 s ，以及一个整数 k 。

首先，用字母在字母表中的位置替换该字母，将 s 转化 为一个整数（也就是，'a' 用 1 替换，'b' 用 2 替换，... 'z' 用 26 替换）。接着，将整数 转换 为其 各位数字之和 。共重复 转换 操作 k 次 。

例如，如果 s = "zbax" 且 k = 2 ，那么执行下述步骤后得到的结果是整数 8 ：

转化："zbax" ➝ "(26)(2)(1)(24)" ➝ "262124" ➝ 262124
转换 #1：262124 ➝ 2 + 6 + 2 + 1 + 2 + 4 ➝ 17
转换 #2：17 ➝ 1 + 7 ➝ 8
返回执行上述操作后得到的结果整数。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/sum-of-digits-of-string-after-convert


## 示例
由于是leetcode周赛，所以输入输出就不用自己阐述了

输入：s = "leetcode", k = 2
输出：6
解释：操作如下：
- 转化："leetcode" ➝ "(12)(5)(5)(20)(3)(15)(4)(5)" ➝ "12552031545" ➝ 12552031545
- 转换 #1：12552031545 ➝ 1 + 2 + 5 + 5 + 2 + 0 + 3 + 1 + 5 + 4 + 5 ➝ 33
- 转换 #2：33 ➝ 3 + 3 ➝ 6
    因此，结果整数为 6 。

## AC代码
```cpp
class Solution {
public:
    int getLucky(string s, int k) {
        int t = 0;
        for (char c: s) {
            int x = c - 'a' + 1;
            t += x % 10;
            t += x / 10;
        }
        for (int i = 1;i < k;++i) {
            int r = 0;
            while (t > 0) {
                r += t % 10;
                t /= 10;
            }
            t = r;
        }
        return t;
    }
};
```
提示：
将原字符串进行一次处理，之后的处理全部转换为int类型变量处理。
# B.子字符串突变后可能得到的最大整数
给你一个字符串 num ，该字符串表示一个大整数。另给你一个长度为 10 且 下标从 0  开始 的整数数组 change ，该数组将 0-9 中的每个数字映射到另一个数字。更规范的说法是，数字 d 映射为数字 change[d] 。

你可以选择 突变  num 的任一子字符串。突变 子字符串意味着将每位数字 num[i] 替换为该数字在 change 中的映射（也就是说，将 num[i] 替换为 change[num[i]]）。

请你找出在对 num 的任一子字符串执行突变操作（也可以不执行）后，可能得到的 最大整数 ，并用字符串表示返回。

子字符串 是字符串中的一个连续序列。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/largest-number-after-mutating-substring

## 示例
输入：num = "021", change = [9,4,3,5,7,2,1,9,0,6]
输出："934"
解释：替换子字符串 "021"：
- 0 映射为 change[0] = 9 。
- 2 映射为 change[2] = 3 。
- 1 映射为 change[1] = 4 。
因此，"021" 变为 "934" 。
"934" 是可以构造的最大整数，所以返回它的字符串表示。
## AC代码一
```cpp
class Solution {
public:
    string maximumNumber(string num, vector<int>& change) {
        int n = num.size();
        int hash[100005] = {0};
        int flag = 1;
        for(int i = 0;i < num.size();i++){
            if(flag == 1 || (i > 0 && hash[i-1] >= 1)){
                if(num[i] - '0' < change[num[i] - '0']){ 
                    flag = 0;
                    num[i] = change[num[i] - '0'] + '0';
                    hash[i]++;
                }else if(num[i] - '0' == change[num[i] - '0'])
                         hash[i]++;
            }
        }
        return num;
    }
};
```
提示：
子字符串进行操作，且寻找最大值，由前往后判断，当且仅当只有连续的才能进行改变。
## AC代码二
```cpp
class Solution {
public:
    string maximumNumber(string num, vector<int>& change) {
        int n = num.size();
        bool start = false;
        for (int i=0; i<n; ++i) {
            int x = num[i] - '0';
            if (change[x] > x) {
                start = true;
            }
            if (change[x] < x) {
                if (start) return num;   
            }
            if (start) {
                num[i] = '0' + change[x];
            }
        }
        return num;
    }
};
```
# C.最大兼容性评分和
有一份由 n 个问题组成的调查问卷，每个问题的答案要么是 0（no，否），要么是 1（yes，是）。

这份调查问卷被分发给 m 名学生和 m 名导师，学生和导师的编号都是从 0 到 m - 1 。学生的答案用一个二维整数数组 students 表示，其中 students[i] 是一个整数数组，包含第 i 名学生对调查问卷给出的答案（下标从 0 开始）。导师的答案用一个二维整数数组 mentors 表示，其中 mentors[j] 是一个整数数组，包含第 j 名导师对调查问卷给出的答案（下标从 0 开始）。

每个学生都会被分配给 一名 导师，而每位导师也会分配到 一名 学生。配对的学生与导师之间的兼容性评分等于学生和导师答案相同的次数。

例如，学生答案为[1, 0, 1] 而导师答案为 [0, 0, 1] ，那么他们的兼容性评分为 2 ，因为只有第二个和第三个答案相同。
请你找出最优的学生与导师的配对方案，以 最大程度上 提高 兼容性评分和 。

给你 students 和 mentors ，返回可以得到的 最大兼容性评分和 。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/maximum-compatibility-score-sum

## 示例
输入：students = [[[[1,1,0],[1,0,1],[0,0,1]]]], mentors = [[[[1,0,0],[0,0,1],[1,1,0]]]]
输出：8
解释：按下述方式分配学生和导师：
- 学生 0 分配给导师 2 ，兼容性评分为 3 。
- 学生 1 分配给导师 0 ，兼容性评分为 2 。
- 学生 2 分配给导师 1 ，兼容性评分为 3 。
最大兼容性评分和为 3 + 2 + 3 = 8 。
## AC代码
```cpp
class Solution {
public:
    int maxCompatibilitySum(vector<vector<int>>& a, vector<vector<int>>& b) {
        int n = a.size(), m = a[0].size();
        vector<int> c(n), d(n);
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j)
                c[i] |= a[i][j] << j, d[i] |= b[i][j] << j;
        vector<int> e(n);
        iota(e.begin(), e.end(), 0);
        int ans = 0;
        do {
            int t = 0;
            for (int i = 0; i < n; ++i)
                t += m - __builtin_popcount(c[i] ^ d[e[i]]);
            ans = max(ans, t);
        } while (next_permutation(e.begin(), e.end()));
        return ans;
    }
};
```
D.删除系统中的重复文件夹
由于一个漏洞，文件系统中存在许多重复文件夹。给你一个二维数组 paths，其中 paths[i] 是一个表示文件系统中第 i 个文件夹的绝对路径的数组。

例如，["one", "two", "three"] 表示路径 "/one/two/three" 。
如果两个文件夹（不需要在同一层级）包含 非空且相同的 子文件夹 集合 并具有相同的子文件夹结构，则认为这两个文件夹是相同文件夹。相同文件夹的根层级 不 需要相同。如果存在两个（或两个以上）相同 文件夹，则需要将这些文件夹和所有它们的子文件夹 标记 为待删除。

例如，下面文件结构中的文件夹 "/a" 和 "/b" 相同。它们（以及它们的子文件夹）应该被 全部 标记为待删除：
- /a
- /a/x
- /a/x/y
- /a/z
- /b
- /b/x
- /b/x/y
- /b/z

然而，如果文件结构中还包含路径 "/b/w" ，那么文件夹 "/a" 和 "/b" 就不相同。注意，即便添加了新的文件夹 "/b/w" ，仍然认为 "/a/x" 和 "/b/x" 相同。
一旦所有的相同文件夹和它们的子文件夹都被标记为待删除，文件系统将会 删除 所有上述文件夹。文件系统只会执行一次删除操作。执行完这一次删除操作后，不会删除新出现的相同文件夹。

返回二维数组 ans ，该数组包含删除所有标记文件夹之后剩余文件夹的路径。路径可以按 任意顺序 返回。

- 来源：力扣（LeetCode）
- 链接：https://leetcode-cn.com/problems/delete-duplicate-folders-in-system
## 示例
![这是图片](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder1.jpg "Magic Gardens")

输入：paths = [["a"],["c"],["d"],["a","b"],["c","b"],["d","a"]]
输出：[["d"],["d","a"]]
解释：文件结构如上所示。
文件夹 "/a" 和 "/c"（以及它们的子文件夹）都会被标记为待删除，因为它们都包含名为 "b" 的空文件夹。

## AC代码
```cpp
class Solution {
public:
    vector<vector<string>> deleteDuplicateFolder(vector<vector<string>>& a) {
        int n = a.size();
        sort(a.begin(), a.end());
        vector<map<string, int>> b(1);
        for (auto& x: a){
            int u = 0;
            for (int i = 0; i + 1 < x.size(); ++i)
                u = b[u][x[i]];
            b[u][x.back()] = b.size();
            b.emplace_back();
        }
        
        vector<int> c(n + 1, 0);
        map<map<string, int>, int> d;
        vector<int> mark(n + 1, 0);
        d[{}] = 0;
        for (int i = n; i > 0; --i){
            if (b[i].empty()) c[i] = 0;
            else {
                map<string, int> x;
                for (auto& pr: b[i])
                    x.emplace(pr.first, c[pr.second]);
                if (d.count(x))
                    c[i] = d[x];
                else
                    c[i] = d.size(), d[x] = c[i];
            }
            printf("%d %d\n", i, c[i]);
        }
        
        map<int, int> lst;
        for (int i = 1; i <= n; ++i) ++lst[c[i]];
        for (int i = 1; i <= n; ++i){
            if (!mark[i] && c[i] > 0 && lst[c[i]] > 1)
                mark[i] = 1;
            
            if (mark[i])
            for (auto& pr: b[i])
                mark[pr.second] = 1;
        }
        vector<vector<string>> ans;
        for (int i = 1; i <= n; ++i)
            if (!mark[i])
                ans.push_back(a[i - 1]);
        return ans;
    }
};
```
# 总结
A,B两题较为基础
C题数量级较小，可以暴力解法。
D题难度较大，放弃。