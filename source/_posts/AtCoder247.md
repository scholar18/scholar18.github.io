---
title: AtCode247
date: 2022-4-18 10:17:00
author: scholar
img: ../images/ACM.jpg
hide: false
summary: abc247
---
# AtCode247th
## A.Move Right
### Problem Address
https://atcoder.jp/contests/abc247/tasks/abc247_a
### 题意
你有四个空间,每个空间有1 或 0, 现将每个空间整体向后移动一位，空余空间用**0**填充。

### AC code
```cpp
#include <bits/stdc++.h>
#define x first
#define y second 
using namespace std;
typedef pair<int, int> PII;
typedef long long ll;
#define endl "\n"

int main()
{
    cin.tie(0)->sync_with_stdio(0);

    string s;
    cin >> s;
    cout << "0" + s.substr(0, 3);
    return 0;
}
// substr 截取字符串的一段
```
## B.Unique Nicknames
### Problem AAddress
https://atcoder.jp/contests/abc247/tasks/abc247_b
### 题意
每个人有一个姓氏，一个名字。

我们想要给每个人去一个绰号，绰号只能是这个人的姓氏或名字，且每个人的绰号不能与他人的名字和姓氏相同。
### AC code
```cpp
#include <bits/stdc++.h>
#define x first
#define y second 
using namespace std;
typedef pair<int, int> PII;
typedef long long ll;
#define endl "\n"

int main()
{
    cin.tie(0)->sync_with_stdio(0);

    int n;
    cin >> n;
    vector<string> s(n), t(n);
    unordered_map<string, int> mp;
    for (int i = 0; i < n; i ++){
        cin >> s[i] >> t[i];
        if(s[i] == t[i]) mp[s[i]] ++;
        else mp[s[i]] ++, mp[t[i]] ++;
    }
    bool flag = 1;
    for (int i = 0 ; i < n; i++){
        if(mp[s[i]] == 1 || mp[t[i]] == 1) flag = 1;
        else {
            flag = 0;
            break;
        } 
    }
    if(flag == 0) cout << "No";
    else cout << "Yes";
    return 0;
}
```

## C.1 2 1 3 1 2 1
### Problem Address
https://atcoder.jp/contests/abc247/tasks/abc247_c
### 题意
有一字符串S, 数组中每个元素满足这样的条件
- s[1] = 1;
- s[i] = s[i - 1] + i + s[i - 1];
### AC code
```cpp
#include <bits/stdc++.h>
#define x first
#define y second 
using namespace std;
typedef pair<int, int> PII;
typedef long long ll;
#define endl "\n"

int main()
{
    cin.tie(0)->sync_with_stdio(0);

    int n;
    cin >> n;
    vector<string> s(n + 1);
    s[1] = "1";
    for (int i = 1; i <= n; i ++){
        s[i] = s[i - 1] + to_string(i) + " " + s[i - 1];
    }
    cout << s[n];
    return 0;
}
```

## D.Cylinder
### Problem Address
https://atcoder.jp/contests/abc247/tasks/abc247_d
### 题意
有一个管道,能进行两次操作。
- 向管道的末尾放**a**个写着**x**的小球。
- 取出前**c**个小球,并计算取出小球上数字的总和

### AC code
```cpp
#include <bits/stdc++.h>
#define x first
#define y second 
using namespace std;
typedef pair<int, int> PII;
typedef long long ll;
#define endl "\n"

const int N = 100010;
int q, n;

int main()
{
    cin.tie(0)->sync_with_stdio(0);

    cin >> q;
    deque<pair<ll, ll>> p;
    while(q --){
        int k, x, c;
        cin >> k;
        if(k == 1) {
            cin >> x >> c;
            p.push_back({x, c});
        }else {
            cin >> c;
            ll sum = 0;
            while(c){
                for (int i = 0; i < p.size(); i ++){
                    if(p[i].y <= c) {
                        sum += p[i].x * p[i].y;
                        c -= p[i].y; 
                        p.pop_front(); 
                        i = -1;
                    } else sum += p[i].x * c, p[i].y -= c, c = 0;
                    if(c == 0) break;
                    }
            }
            cout << sum << '\n';
        }
    }
    return 0;
}
```
## E. Max Min
### Problem Adress
https://atcoder.jp/contests/abc247/tasks/abc247_e
### 题意
给一个长度为**n**的序列A, 以及两个值**x,y**.
求有多少组(l, r) 满足一下条件

- $1 \le l \le r \le n $ 
- 序列$A_l $ ~  $A_r$中最大值是x, 最小值是y
### 题解
我们将等于x和等于y的下标记录在两个数组中，将大于x and 小于y的数的下标记录在另一个数组中。
之后枚举每一个i，找到i的左边界和右边界
- 对于左边界，它应该等于x或者等于y的下标的最大值
- 对于右边界，它应该是第一个大于x or 小于 y 的一个数
这样我们保证这三个数组都是递增排列的，所以直接二分查找左右边界就行了
### AC code
```cpp
#include <bits/stdc++.h>
using namespace std; 
#define endl "\n"
#define inf 0x3f3f3f3f
#define mod7 1000000007
#define mod9 998244353
#define debug(a) cout << "Debuging...|" << #a << ": " << a << "\n";
typedef pair<int, int> PII;
typedef long long ll;

const int N = 100010;
int main()
{
    cin.tie(0)->sync_with_stdio(0);

    int n, x, y;
    cin >> n >> x >> y;
    vector<int> nums(n + 1), xr, yr, v; 
    for (int i = 1; i <= n; i ++) {
        cin >> nums[i];
        if(nums[i] == x) xr.emplace_back(i);
        if(nums[i] == y) yr.emplace_back(i);
        if(nums[i] < y || nums[i] > x) v.emplace_back(i);
    }
    xr.emplace_back(INT_MAX), yr.emplace_back(INT_MAX);
    v.emplace_back(n + 1);
    ll ans = 0;
    for (int i = 1; i <= n; i ++){
        ll r  = *lower_bound( v.begin(), v.end(),  i);
        ll lx = *lower_bound(xr.begin(), xr.end(), i);
        ll ly = *lower_bound(yr.begin(), yr.end(), i);
        ans += max(0ll, r - max(lx, ly));
    }
    cout << ans << endl;
    return 0;
}
```
