---
title: Codeforces
date: 2022-7-5 22:13:00
author: scholar
img: ../images/ACM.jpg
hide: false
summary: div.2 804
---
# codeforces round #804（div.2）A - C
## A.The Third Three Number Problem
### 题意：
给定一个非负整数n,要求找到任意的非负整数a, b, c.
满足 a ^ b + b ^ c + c ^ a = n.
### 思路：
若二进制上有1，则a, b, c的前一位二进制数应该是1, 1, 0.
所以n的二进制数的第一位一定不是1，既n是奇数，输出 **-1**

    证明奇数不成立：异或运算的奇偶性与加法的奇偶性相同,
    既a ^ b + b ^ c + c ^ a 的奇偶性 == a + b + b + c + c + a
    所以n一定为偶数
然后 0^(n/2) = n/2, 0 ^ 0 = 0, 所以这三个数为 **n/2, 0, 0**.

### 代码：
```cpp
#include <bits/stdc++.h>
using namespace std; 
#define endl "\n"
#define inf 0x3f3f3f3f
#define mod7 1000000007
#define mod9 998244353
#define rep(i,n,m) for(int i=n;i<m;i++)
#define pb push_back
#define debug(a) cout << "Debuging...|" << #a << ": " << a << "\n";
#define f first
#define s second
#define int long long
#define ld long double
#define pi pair<int,int>
#define pld pair<ld,ld>
typedef long long ll;

void solve()
{
    int a, b, c, n;
    cin >> n;
    if(n & 1) {
        cout << -1 << endl;
        return ;
    }
    cout << n / 2 << ' ' << "0 0\n";
}
signed main()
{
    cin.tie(0)->sync_with_stdio(0);

    int _; cin >> _;
    while(_ --){
        solve();
    }
    return 0;
}
```

## B. Almost Ternary Matrix
### 题意：
给定一个n*m的01矩阵, n, m 均为**偶数**。
要求对于每一个方格，在它相邻的方格中**正好**有两个与它不同的邻居。
### 思路：
这题完全没读懂题意当时，一直在想搜索，实则是一个找规律的构造题。
**当n = 2时**， 矩阵为：
**1 0 0 1** 1 0 0 1 ....
**0 1 1 0** 0 1 1 0 ....
我们发现以四个为单位，后面是循环进行的。
**当n = 4时**， 矩阵为：
**1 0 0 1** 1 0 0 1......
**0 1 1 0** 0 1 1 0......
**0 1 1 0** 0 1 1 0......
**1 0 0 1** 1 0 0 1......
矩阵行方面是以四个单位循环，列方面也是以四个单位循环
由此，我们发现：
当i % 4 == 0 || i % 4 == 3时，按照 **1 0 0 1**循环
其中j%4 == 0 || j % 4 == 3时，取1.j % 4 == 2 || j % 4 == 3时，取0
当  i % 4 == 1 || i % 4 == 2时，按照 **0 1 1 0**循环
其中j % 4 == 1 || j % 4 == 2时，取1.j % 4 == 0 || j % 4 == 3时，取0
综上，当i%4 == j % 4 || i % 4 + j % 4 == 3时，取1。其余，取0
### 代码：
```cpp
#include <bits/stdc++.h>
using namespace std; 
#define endl "\n"
#define inf 0x3f3f3f3f
#define mod7 1000000007
#define mod9 998244353
#define rep(i,n,m) for(int i=n;i<m;i++)
#define pb push_back
#define debug(a) cout << "Debuging...|" << #a << ": " << a << "\n";
#define f first
#define s second
#define int long long
#define ld long double
#define pi pair<int,int>
#define pld pair<ld,ld>
typedef long long ll;

int n, m;
void solve()
{
    cin >> n >> m;
    rep(i, 0, n)
        rep(j, 0, m)
            cout << (i % 4 == j % 4 || i % 4 + j % 4 == 3)<< " \n"[j == m - 1]; 
}

signed main()
{
    cin.tie(0)->sync_with_stdio(0);

    int _; cin >> _;
    while(_ --){
        solve();
    }
    return 0;
}
```
## C. The Third Problem
### 题意：
给定n，以及一个从0到n-1的全排列，问有多少个0到n-1的全排列与之相似（包含自己）
相似的含义是，对于全排列中任意一个区间[l,r],区间的MEX都相同
MEX是最小的没有在区间中出现的非负整数
### 思路：
MEX的定义是没有出现在区间中的最小的非负整数，所以我们由0~n-1不断扩展区间来考虑他们的位置。
由**0 ~ n-1**来枚举i，一个数i是否可以移动的准则是：
当i在区间内，该区间**MEX >= i + 1**, 既i在该区间内可以移动的位置有**r - l + 1 - i**个，在该区间内 **>=i**的位置都可以移动。
当i不在区间内，那么i的位置不能改变，因为如果将i与区间的数交换，区间的MEX变化，如果将i与区间外的数交换，同理也会有区间的MEX变化。
### 代码：
```cpp
#include <bits/stdc++.h>
using namespace std; 
#define endl "\n"
#define inf 0x3f3f3f3f
#define mod7 1000000007
#define mod9 998244353
#define rep(i,n,m) for(int i=n;i<m;i++)
#define pb push_back
#define debug(a) cout << "Debuging...|" << #a << ": " << a << "\n";
#define f first
#define s second
#define int long long
#define ld long double
#define pi pair<int,int>
#define pld pair<ld,ld>
typedef long long ll;

void solve()
{
    int n; cin >> n;
    vector<int> a(n + 1), b(n + 1);
    rep(i, 1, n + 1) cin >> a[i], b[a[i]] = i;
    int res = 1;
    int l = b[0], r = b[0]; 
    rep(i, 1, n) {
        if(b[i] < l) l = b[i];
        else if(b[i] > r) r = b[i];
        else res = res * (r - l + 1 - i) % mod7;
    }
    cout << res << endl;
}
signed main()
{
    cin.tie(0)->sync_with_stdio(0);

    int _; cin >> _;
    while(_ --){
        solve();
    }
    return 0;
}
```