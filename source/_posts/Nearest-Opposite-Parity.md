---
title: Nearest Opposite Parity
description: 反向建图+bfs 注意初始化问题
tags:
  - 每日一题
categories:
  - 算法
cover: >-
  https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/%E6%8F%92%E7%94%BB4.jpg
mathjax: true
toc: style_simple
abbrlink: 745661d0
date: 2022-07-13 03:07:58
---
## [题目描述](https://codeforces.com/contest/1272/problem/E)

给定一个长度为 $n$ 的下标从 $1$ 开始的数组 $a$，在位置 $i$ 可以移动到位置 $i−ai  \ \ (1≤i−ai)$ 或 $i+ai \  \ (i+ai≤n)$
现在希望你对于每一个位置 $i \ \ (1≤i≤n)$ 求出从这里出发，抵达任意一个位置 $j$ 使得该位置 $j$ 满足 $a_i \ mod \ 2\ ≠ \ a_j \ mod \ 2$ 的最小步数，如果不存在这样的位置 $j$ 则输出 $-1$
## 思路
很容易想到通过每个位置 $i$ 可以到达的位置 ``建图``

那么问题就转化为

在图中对于每个顶点找到第一个与它奇偶性不同的顶点最少需要几步

这里如果位置 $i$ 可以到达位置 $j$ 那么就向 $j$ 到 $i$ 连一条有向边,并且预处理出如果位置 $i$ 可以一步找到那么就使他的 $d[i] = 1$,并入队进行 $bfs$

考虑到 bfs 可以找到 ``最短路`` ! !,这一点给忽视了

反向建图就是为了运用 ``bfs`` 去更新那些没有初始化的顶点

对于每个顶点i,若与它相邻的 $j$ 等于 $-1$

说明 $j$ 未被初始化,它与 $i$ 奇偶性相同则 

$d[j] = d[i] + 1$,并把 $j$ 入队


## Code
```cpp
vector<int> g[200005];

signed main()
{
    int n;cin>>n;
    vector<int> a(n+1);
    forr(i,1,n) cin >> a[i];
    vector<int> d(n+1,-1);
    queue<int> q;
    forr(i,1,n){
        if(i+a[i] <= n){
            if((a[i] % 2) != (a[i+a[i]]%2)){
                d[i] = 1;
                q.push(i);
            }
            g[i+a[i]].push_back(i);
        }
        if(i-a[i]>=1){
            if((a[i] % 2) != (a[i-a[i]]%2)){
                d[i] = 1;
                q.push(i);
            }
            g[i-a[i]].push_back(i);
        }
    }

    while(q.size()){
        int u = q.front();
        q.pop();
        for(auto v:g[u]){
            if(d[v] == -1){
                d[v] = d[u] + 1;
                q.push(v);
            }    
        }
    }
    forr(i,1,n) cout << d[i] <<" \n"[i==n];
    return 0;
}
```

