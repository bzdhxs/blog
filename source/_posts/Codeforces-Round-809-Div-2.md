---
title: Codeforces Round-#809-(Div. 2)
description: 补题
tags:
  - codeforces
categories:
  - 算法
cover: >-
  https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/%E6%8F%92%E7%94%BB10.png
mathjax: true
toc: style_simple
abbrlink: f94146b4
date: 2022-07-19 22:45:04
---

[Codeforces Round #809 (Div. 2)](https://codeforces.com/contest/1706)

## A .[Another String Minimization Problem](https://codeforces.com/contest/1706/problem/A)

很自然想到，枚举数组取

 $t = min( a[i]  , m+1 -a[i] )$ 

$k = max(a[i],m+1-a[i])$

若 $s[t]$ 不为 $A$, 变为 $A$ 并标记 

$s[t]$ 被标记,则把$s[k]$ 变为 $A$ 并标记

```CPP
void solve(){
    int n,m;
    cin>>n>>m;
    vector<int> a(n+1);
    forr(i,1,n) cin >> a[i];
    string s;
    forr(i,1,m) s+="B";
    //cout << s << endl;
    map<int,int> mp;
    forr(i,1,n){
        int t = min(a[i],m+1-a[i]);
        int k = max(a[i],m+1-a[i]);
        if(mp[t-1]){
            s[k-1] = 'A';
            mp[k-1]++;
        }
        else{
            mp[t-1]++;
            s[t-1] = 'A';
        }
        //cout << s << endl;
    }
    cout << s << endl;
}

```



## B .[Making Towers](https://codeforces.com/contest/1706/problem/B)

找规律

对于数 $i$ ， 当数 $i$ 之间间隔为``偶数``时 ,$ans[i]++$;

```cpp
vector<int> g[100005];
void solve(){
    int n;cin>>n;
    vector<int> a(n+1);
    set<int> s;
    forr(i,1,n) cin >> a[i],g[a[i]].push_back(i),s.insert(a[i]);
    map<int,int> mp;
    for(auto it = s.begin();it!=s.end();it++){
        int t = *it;
        mp[t] = 1;
        for(int i = 1;i < g[t].size();i++){
            int tt = g[t][i] - g[t][i-1] - 1;
            if( (tt%2)==0) mp[t]++;
        }
    }
    forr(i,1,n) cout << mp[i] <<" \n"[i==n];
    for(auto it = s.begin();it!=s.end();it++) g[*it].clear();
}

```



## C .[Qpwoeirut And The City](https://codeforces.com/contest/1706/problem/C)

先要求峰``最多``再要求添加``最少``

奇数答案一定隔一个判断一次，贡献一次答案

考虑偶数

​	偶数在满足峰最多的前提，有一次``可以``隔两个选并且``只有``一个位置,否则不可能取到最多峰

​	那么枚举哪个位置隔二个，很多题解都是通过两个前缀来做

​	官方 tutorial 方法更容易理解

![image-20220719221721952](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220719221721952.png)

​	

```cpp
int a[100005];
int cal(int x){
    return max(0ll,(max(a[x-1],a[x+1])+1-a[x]));
}

void solve(){
    int n;
    cin >> n;
    forr(i,1,n) cin >> a[i];
    int res = 0;
    if(n&1){
        for(int i = 2;i <= n;i+=2){
            if(a[i] > a[i-1] && a[i] > a[i+1]) continue;
            else {
                res += max(a[i-1],a[i+1]) + 1-a[i];
            }
        }
            cout << res << endl;
    }
    else{
            for(int i = 2; i < n;i+=2){
                if(a[i] > a[i-1] && a[i] > a[i+1]) continue;
                else {
                    res += max(a[i-1],a[i+1]) + 1-a[i];
                }                
            }
            int sum = res;
            for(int i = n-1;i>1;i-=2){
                res -= cal(i-1);
                res += cal(i);
                sum = min(res,sum);
            }
            cout << sum << endl;
        }
}

```



## D1 .[Chopping Carrots (Easy Version)](https://codeforces.com/contest/1706/problem/D1)

$n <= 3000 $

考虑 $\min\limits_{1<=i<=n}(\lfloor \frac{a_i}{p_i} \rfloor) \  = \  v$ 

分析可知 $v ∈ [0,a_1]$ 

反证

​	若 $v ∈ [a_2,a_n]$  因为 $\min\limits_{1<=i<=n}(\lfloor \frac{a_i}{p_i} \rfloor) \  = \  v$ ， 那么对于 $x \ =\  [a_1,v-1]$  , 总有 $\frac{x}{p_i} < v$ 

故  $v ∈ [0,a_1]$ 

那么枚举 $v$, 计算出 $p_i = min(k,a_i/v)$ ,再用计算出的 $p_i$ 求出 $\max\limits_{1<=i<=n}(\lfloor \frac{a_i}{p_i} \rfloor)$  和 $\min\limits_{1<=i<=n}(\lfloor \frac{a_i}{p_i} \rfloor)$ 

做差求最大的差值  $res$

 $O(n*a_i)$

```cpp
void solve(){
    int n,k;
    cin>>n>>k;
    vector<int> a(n+1);
    forr(i,1,n) cin >> a[i];
    int mx = -inf,mi = inf;
    int res = inf;
    for(int v = 0;v <= a[1];v++){
        int p;
        forr(i,1,n){
            if(!v) p = k;
            else p = min(k,a[i]/v);
            mx = max(mx,a[i]/p);
            mi = min(mi,a[i]/p);
        }
        // cout << mx << " " << mi << endl;
        // cout << res << endl;
        res = min(res,mx-mi);
        mx = -inf,mi = inf;
    }
    cout << res << endl;
}
```



## E .[Qpwoeirut and Vertices](https://codeforces.com/contest/1706/problem/E)

[Kruskal 重构树](https://bzdhxs.github.io/2022/07/19/Kruskal-%E9%87%8D%E6%9E%84%E6%A0%91/)

给一个区间$[l,r]$

要求的就是这个区间的子区间 （ $[a,b]\,\ l<=a<=b<=r$  $a\rightarrow b$ 的所有路径最长边权的``最小值`` ）的``最大值``

$[a,b]\,\ l<=a<=b<=r$  $a\rightarrow b$ 的所有路径最长边权的``最小值`` ，可以用``Kruskal重构树``直接求出

然后就是对 [l,r] 的一次区间求和, 因为是静态的可使用 $ ST表 $ $O(n)$ 预处理

注意各种数组的``初始化``问题



```cpp
int n, m, q;
struct node
{
    int u, v, w;
    bool operator<(const node &t) const
    {
        return w < t.w;
    }
} edge[400005];
struct nod
{
    int to, ne;
} e[400005<<1];
int h[400005<<1], cnt = 1;
void add(int u, int v)
{
    e[++cnt] = {v, h[u]};
    h[u] = cnt;
}
int pre[400005<<1];
int tot;
int val[400005];
int d[400005<<1];
int f[200005][21]; 

int find(int x)
{
    return x == pre[x] ? x : pre[x] = find(pre[x]);
}

void dfs(int u, int fa)
{
    d[u] = d[fa] + 1;
    // cout <<u <<" "<<  d[u] << endl;
    for (int i = h[u]; i; i = e[i].ne)
    {
        int v = e[i].to;
        if (v == fa)
            continue;
        f[v][0] = u;
        forr(k, 1, 20) 
            if (f[v][k - 1])
                f[v][k] = f[f[v][k - 1]][k - 1];
            else 
                break;
        dfs(v, u);
    }
}

int lca(int a, int b)
{
    if (d[a] > d[b])
        swap(a, b);
    for (int i = 20; i >= 0; i--)
        if (d[f[b][i]] >= d[a])
            b = f[b][i];
    if (a == b)
        return a;
    for (int i = 20; i >= 0; i--)
        if (f[a][i] != f[b][i])
            a = f[a][i], b = f[b][i];
    return f[a][0];
}

void kruskal()
{
    sort(edge + 1, edge + 1 + m);
    forr(i, 1, n) pre[i] = i,d[i] = 0;
    tot = n;
    forr(i, 1, m)
    {
        int a = find(edge[i].u), b = find(edge[i].v);
        if (a != b)
        {
            val[++tot] = edge[i].w;
            d[tot] = 0;
            pre[tot] = pre[a] = pre[b] = tot;
            add(tot, a);
            add(a, tot);
            add(b, tot);
            add(tot, b);
        }
    }
    for (int i = tot; i >= 1; i--)
    {
        if (!d[i]){
            dfs(i, -1);
        }
    }
}

int dis[200005];
int ff[200005][21];
void ST(){
    // 不用初始化
    forr(i,1,n-1) ff[i][0] = dis[i];
    forr(i,1,20)for(int j = 1;j+(1<<i)-1 <= n-1;j++){
        ff[j][i] = max(ff[j][i-1],ff[j+(1<<(i-1))][i-1]);
    }
}


int query(int l,int r){
	int s = __lg(r-l+1);
	return max(ff[l][s],ff[r-(1<<s)+1][s]);
}
void solve()
{

    cin >> n >> m >> q;
    cnt = 1;
    forr(i,1,2*n) h[i] = 0;
    forr(i,1,2*n) forr(j,1,20) f[i][j] = 0;
    cnt = 1;
    forr(i, 1, m)
    {
        int u, v;
        cin >> u >> v;
        edge[i] = {u, v, i};
    }
    kruskal();

    for(int i = 1;i<n;i++){
        int t = lca(i,i+1);
        dis[i] = val[t];
    }
    ST();
    while (q--)
    {
        int l, r;
        cin >> l >> r;
        if(l==r) cout << 0 <<" ";
        else cout << query(l,r-1) << " ";
    }
    cout << endl;
}

```

