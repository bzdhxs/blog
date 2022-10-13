---
title: Mouse Hunt
tags:
  - 每日一题
categories:
  - 算法
mathjax: true
toc: style_simple
cover: 'https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/96.JPG'
description: 基环树+思维
abbrlink: b5067d9
date: 2022-09-22 01:13:38
---



Mouse Hunt

# [题目大意](http://oj.daimayuan.top/problem/848)

![题意](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220921123856286.png)



根据题意可知，因为每个点的出度为 $1$，抽象成图就是一个``基环树``，然后求一个环上最小的点值

# 分析



现在思考如何找到环和求环上的最小点权

假设从 $i$ 点开始遍历图，  $vis$  去记录当前点是从哪个点遍历来的且可以表示是否被访问

 在遍历的过程中，遇到没有遍历过的点压入栈中

遍历时，判断当前点 $x$ 的邻接点 $p[x]$ 的 $vis$ 是否等于 $i$

如果等于 $i$ ,说明下一步将会再次遍历到 $p[x]$，这样就出现了一个环

找到环后，就开始进行出栈操作，对于当前栈顶 $top$

我们用 $stk[top+1]$ 来看是不是等于 $p[x]$ 的原因是

 ```cpp
 while(stk[top+1] != p[x]){
	minn = min(minn,w[stk[top]]);
 	top--;
}
 ```

为了方便处理``环的开头``

#  Code

```cpp
int n;
int w[200005],p[200005];
int vis[200005];
int stk[200005];

signed main()
{
    cin >> n;

    for(int i = 1;i <= n;i++) cin >> w[i];
    for(int i = 1;i <= n;i++) cin >> p[i];
    int res = 0;
    for(int i = 1;i <= n;i++){
        int top  = 0;
        if(!vis[i]){
            for(int x = i;!vis[x];x = p[x]){
                vis[x] = i;
                stk[++top] = x;
                if(vis[p[x]] == i){
                    int minn = inf;
                    while(stk[top+1] != p[x]){
                        minn = min(minn,w[stk[top]]);
                        top--;
                    }
                    res += minn;
                }
            }
        }    
    }
    cout << res << endl;

    return 0;
}
```

