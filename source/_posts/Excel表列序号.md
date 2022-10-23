---
title: Excel表列序号
date: 2021-07-29 21:08:00
author: 曹毅
---
# Excel表列序号
给你一个字符串 columnTitle ，表示 Excel 表格中的列名称。返回该列名称对应的列序号。
如：
- A = 1
- B = 2
- C = 3
- .....
- .....
- .....

## 示例
输入: columnTitle = "A"
输出: 1
## 思路
显然易见这是个进制转换问题，将26进制转换为10进制

注意，数量范围应为long long
## AC代码
```cpp
    include<bits/stdc++.h>
    using namespace std;
    int main()
    {
        string s;
        cin >> s;
        long long ans = 0;
        for(auto i:columnTitle){
            ans = ans * 26 + i - 'A' + 1;
        }
        cout << ans << endl;
    }
```