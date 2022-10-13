---
title: Kruskal 重构树
tags:
  - Template
categories: 
  - 算法
toc: style_simple
description: Kruskal 重构树 学习记录
cover: 
  https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/%E6%8F%92%E7%94%BB24.jpg
mathjax: true
abbrlink: fbb465cf
date: 2022-07-19 21:51:10
---
# Kruskal 重构树



## 引入

[**[NOIP2013 提高组] 货车运输**](https://www.luogu.com.cn/problem/P1967)

题目大意：n 个点 m 条无向边的图，k 个询问，每次询问从 u 到 v 的所有路径中，``最长的边的最小值``。

$1≤n≤15000,1≤m≤30000,1≤k≤20000。$





## 理解



引入的问题就是 Kruskal 重构树算法的一个应用

​	用来询问一张图中  $u$  到  $v$ 的所有路径最长边的最小值

我们先来了解一下 Kruskal 重构树的一些性质

Kruskal 重构树由原图的$n$个点，新添 $n-1$ 个点和 $2n-2$ 条边构成的树。

（以下讨论的Kruskal重构树都是最小Kruskal重构树，最大Kruskal重构树反之亦然）

下面是一个图和它的Kruskal重构树：（圆点是原图中的点，方点是新加的点）

![1418922-20180724190930874-672302246](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/1418922-20180724190930874-672302246.png)

它的性质有：

1. 一棵二叉树
2. **叶子结点都是原图中的点**，没有点权；**非叶子结点都是新加的点**，有点权
3. 旧点 $u,v$ 两点间（不包括 u,v）的所有节点（都是新点）的点权最大值为原图中 $u,v$ 两点间所有路径的最长边的最小值
4. 新点构成一个堆（不一定是二叉堆），在**最小Kruskal重构树中是大根堆**（最大Kruskal重构树中是小根堆）
5. 结合性质$3$和$4$，旧点 $u,v$ **两点的LCA（是新点）权值**为原图中 $u,v$ 两点间所有路径的最长边的最小值

比如点 $1,3$ 的LCA权值为 $3$，恰好是原图中 $1,3$ 两点间所有路径的最长边的最小值 $边 (3,5)$。



$ Kruskal $ 重构树怎么求呢？

它的思想是这样的：

**在运行Kruskal算法的过程中，对于两个可以合并的节点(x,y)，断开其中的连边，并新建一个节点T，把TT向(x,y)连边作为他们的父亲，同时把(x,y)之间的边权当做T的点权**

比如说

![krusal](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/krusal.jpg)

重构之后是这样的：

​																						  

​                                         ![kruskal](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/kruskal.jpg)   				



这样我们得到了一个新的树

具体操作步骤

1. 找到一条边权最小的，且没有被遍历过的边。
2. 如果这条边连接的两个点目前没有在一个联通块，那么新建一个节点，点权为该边的边权，左右儿子设为**这两个点的最远祖先**。（通过设置儿子为最远祖先合并联通块且不破坏性质）
3. 重复1,2直到遍历完所有的边。

下图中红点是该边连接的两个点，绿点和绿边是添加的点和边

![Kruskal](https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/1610347-20200113162019246-922619957.png)

​	

## 代码实现

在 $Kruskal$ 加边的过程中，当两点 $u,v$ 不在同一集合内，新建立一个结点 $tot$，该点点权为 $u\rightarrow v$ 的边权，把$u,v$的父亲置为 $tot$,实现 tot 如下

```cpp
void kruskal()
{
    sort(e + 1, e + 1 + m);
    forr(i, 1, n) pre[i] = i; // 祖先结点初始化
    tot = n;
    forr(i, 1, m)
    {
        int a = find(e[i].u), b = find(e[i].v);
        if (a != b)
        {
            val[++tot] = e[i].w; // 新结点点权
            pre[tot] = pre[a] = pre[b] = tot; // 置父亲节点
            add(tot, a);add(a, tot); //	加边根据个人习惯可加双向边,也可加单向边需要配合 vis[]
            add(b, tot);add(tot, b);
        }
    }
    //将无根树转化成有根树 (重构后tot是根节点,倒着dfs才能保证求出的lca是正确的)
    // 同时求 dep 深度 和 f[][] 数组 
    for (int i = tot; i >= 1; i--)
    {
        if (!d[i])
            dfs(i, -1);
    }
}
```



使用倍增求 $LCA$

```cpp
void dfs(int u, int fa)
{
    d[u] = d[fa] + 1; // 深度
    for (int i = h[u]; i; i = edge[i].ne)
    {
        int v = edge[i].to;
        if (v == fa)
            continue;
        f[v][0] = u; // 倍增数组,在dfs过程中求出祖先
        forr(k, 1, 20) 
            if(f[v][k - 1])
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

```



#### Code

```cpp
typedef long long ll;
int pri[16] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53};
const int inf = 0x3f3f3f3f;
const int N = 1e6 + 10;
int n, m;
struct node
{
    int u, v, w;
    bool operator<(const node &t) const
    {
        return w > t.w;
    }
} e[50005];
struct nod
{
    int to, ne;
} edge[20005];
int h[20005], cnt = 1;
int pre[20005];
int val[20005];
int d[20005];
int f[20005][21];
int tot;
void add(int u, int v)
{
    edge[++cnt] = {v, h[u]};
    h[u] = cnt;
}

int find(int x)
{
    return x == pre[x] ? x : pre[x] = find(pre[x]);
}

void dfs(int u, int fa)
{
    d[u] = d[fa] + 1;
    //cout <<u <<" "<<  d[u] << endl;
    for (int i = h[u]; i; i = edge[i].ne)
    {
        int v = edge[i].to;
        if (v == fa)
            continue;
        f[v][0] = u;
        forr(k, 1, 20) 
            if(f[v][k - 1])
                f[v][k] = f[f[v][k - 1]][k - 1];
            else 
                break;
        dfs(v, u);
    }
}

void kruskal()
{
    sort(e + 1, e + 1 + m);
    forr(i, 1, n) pre[i] = i;
    tot = n;
    forr(i, 1, m)
    {
        int a = find(e[i].u), b = find(e[i].v);
        if (a != b)
        {
            val[++tot] = e[i].w;
            pre[tot] = pre[a] = pre[b] = tot;
            add(tot, a);
            add(a, tot);
            add(tot, b);
            add(b, tot);
        }
    }
    for (int i = tot; i >= 1; i--)
    {
        if (!d[i])
            dfs(i, -1);
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

signed main()
{
   // _orz;
    cin >> n >> m;
    forr(i, 1, m)
    {
        int u, v, w;
        cin >> u >> v >> w;
        e[i] = {u, v, w};
    }
    kruskal();
    int q;cin>>q;
    while(q--){
        int x,y;cin>>x>>y;
        if(find(x)^find(y)) puts("-1");
        else cout << val[lca(x,y)] << endl;
    }
    return 0;
}
```



## 参考资料

[参考资料1](https://www.cnblogs.com/1000Suns/p/9360558.html)

[参考资料2](https://blog.nowcoder.net/n/14d05b05e06f4029b0d0ec4af3737f95)

感谢分享知识！