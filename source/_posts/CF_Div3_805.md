---
title: Codeforces
date: 2022-7-13 22:30:00
author: scholar
img: ../images/ACM.jpg
hide: false
summary: Div.3 805
---

# codeforces round #805（div.3）A - F
比赛结束就想写了, 纪念第一次写五题, 但最近在学其他的, 现在才写完qaq.
## A. Round Down the Price
### 题意：
给定t个数，对于每个数n，我们想可以找到一个数X，满足n - x是不大于n的最大的10的幂次数
既101 = 1， 10 = 0， 98 = 88 
### 思路：
对于长度为x的n，我们要求的值是n - pow(10, x - 1)
### 代码：
```cpp
int count(int x)
{
    int cnt = 0;
    while(x) x /= 10, cnt ++;
    return cnt; 
}
void solve()
{
    int n; cin >> n;
    int idx = count(n);
    int res = pow(10, idx - 1);
    if(n == res) cout << 0 << endl;
    else cout << n - res << endl;
}

```
## B.  Polycarp Writes a String from Memory
### 题意：
对于一个字符串s，我们一次可以记住三个不同的字符，但一天之后会遗忘，现在想把这个字符串写出来。
问：多少天可以写出这个字符串
### 思路：
随着循环，以三个不同字符为单位记录为t，ans =+ t
输出ans/3 + ans % 3 == 0?0:1
### 代码：
```cpp
void solve()
{
    string s;
    cin >> s;
    int res = 0;
    map<char, int> mp;
    int t = 0;
    rep(i, 0, s.size()){
        if(mp.find(s[i]) == mp.end()) {
            if(t == 3){
                mp.clear();
                t = 0;
            } 
            mp[s[i]] ++;
            t ++;
            res ++;
        }   
    }
    cout << res/3 + (res % 3 == 0?0:1) << endl;
}

```
## C. Train and Queries
### 题意：
给定n个数，m次查询，每次查询a, b.
判断a出现的位置是否在b出现的位置前面(n个数可能重复)
### 思路：
记录每个数第一次出现的位置，之后进行比较，满足条件输出**YES**,否则输出**NO**
### 代码：
```cpp
void solve()
{
    int n, k;
    cin >> n >> k;
    vector<int> a(n + 1);
    map<int, int> f, l;
    rep(i, 1, n + 1) {
        cin >> a[i];
        if(f.find(a[i]) == f.end()) f[a[i]] = i;
        else f[a[i]] = min(f[a[i]], i);
        if(l.find(a[i]) == l.end()) l[a[i]] = i;
        else l[a[i]] = max(l[a[i]], i);
    }
    while(k --){
        int x, y; cin >> x >> y;
        
        if(f[x] < l[y] && f[x] != 0 && l[y] != 0)  cout << "YES\n";
        else cout  << "NO\n";
    }
}

```
## D. Not a Cheap String
### 题意：
给定一个字符串s(由小写字母组成), 该字符串的价值是其中字符在字母中位置(1~26)的和.
给定k，求出我们删除多少字符后，字符串的价值小于等于k.
### 思路：
我们每次删除价值最大的字符，就行了。
首先记录每个字符出现的位置，记录下来，从'z'~'a'开始删除，直至不大于k。
### 代码：
```cpp
void solve()
{
    string a; cin >> a;
    int p, n; cin >> p;
    n = a.size();
    int res = 0;
    vector<vector<int>> idx(27);
    rep(i, 0, n){
        idx[a[i] - 'a'].push_back(i);
        res += a[i] - 'a' + 1;
    }
    if(res <= p) {
        cout << a << endl;
        return ;
    }
    map<int, int> mp; 
    for (int i = 25; i >= 0; i --){
        for (int j = 0; j < idx[i].size(); j ++){
            mp[idx[i][j]] ++;
            res -= i + 1;
            if(res <= p){
                rep(k, 0, n){
                    if(mp.find(k) == mp.end())
                        cout << a[k];

                }
                cout << endl;
                return ;
            }
        }
    }
}
```
## E. Split Into Two Sets
### 题意：
有一n个多米诺骨牌，每个牌有正反两面，每面都有一个数字(1~n).
现在你需要把骨牌分成两堆，使得每一个堆里面都没有重复的数字。问是否可以分成两堆。
### 思路：
感觉以前见过这一题，做题时还是没想起来怎么写的.
如果一个骨牌上出现了两个相同的数字，那么一定不可以。
如果同一个数字出现了两次以上，那么也不可以。
我们根据不同骨牌建图，若一个数x在i,j骨牌上都出现了, 那么i, j一定不能放在同一堆.
我们对i和j建立无向边, 这样我们就得到了一个无向图.
我们对其进行黑白染色, 颜色相同放在一起即可.
Tip:染色是可以1, 2染色, 因为3 - 1 = 2, 3 - 2 = 1, 可以相互转换.

### 代码：
```cpp
const int N = 2e5 + 5;
vector <int> adj[N];
queue <int> q;
int col[N];
int n, ans;

void solve()
{
    cin >> n;
    for (int i = 1; i <= n; i++) adj[i].clear(), col[i] = 0;
        for (int i = 1; i <= n; i++) {
            int u, v;
            cin >> u >> v;
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        ans = 1;
        for (int _ = 1; _ <= n; _++) {
            if (col[_]) continue;
            q.push(_); col[_] = 1;
            while (!q.empty()) {
                int u = q.front(); q.pop();
                for (auto v : adj[u]) {
                    if (!col[v]) {
                        col[v] = 3 - col[u];
                        q.push(v);
                    } else if (col[u] == col[v]) ans = 0;
                }
            }
        }
        for (int i = 1; i <= n; i++) {
            if ((int)adj[i].size() > 2) {
                ans = 0;
                break;
            }
        }
        printf("%s\n", ans ? "YES" : "NO");
    
}

```

## F. 
### 题意：
有长度为n的两个多重集合a, b.($n \le 10^5$)
我们可以对b进行如下操作:
- b[i] = b[i] * 2;
- b[i] = b[i] / 2;(向下取整)
问最少进行多少次操作后可以 使**a = b**, 若不行输出 **-1**.
### 思路：
如果一个b可以通过多次*2或/2操作变成a,同理a可以多次/2 或 *2变成b.
所以我们先把a, b化为最简, 既其中只包含奇数.
若a, b可以匹配, 那么肯定是a中最大的和b中最大的匹配。
但要注意b还可以 /2, 既现在a, b中最大的数可能不是互相匹配的.
既若匹配不成功, 将b **/2**直至变成奇数
若匹配成功, 将这两个最大值弹出
### 代码：
```cpp
void solve()
{
    int n; cin >> n;
    int x;
    priority_queue<int> qa, qb;
    rep(i, 0, n) {
        cin >> x;
        while(x % 2 == 0) x /= 2;
        qa.push(x);
    }
    rep(i, 0, n) {
        cin >> x;
        while(x % 2 == 0) x /= 2;
        qb.push(x);
    }
    while(!qb.empty())
    {
        int x = qb.top(); qb.pop();
        int y = qa.top(); qa.pop();
        if(x != y) {
            x /= 2;
            if(x == 0){
                cout << "NO\n";
                return ;
            }
            qb.push(x);
            qa.push(y);
        }
    }
    cout << "YES\n"; 
}
```


