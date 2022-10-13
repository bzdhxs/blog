---
title: 不朴素的数列（Bonus）
tags:
  - 每日一题
categories:
  - 算法
mathjax: true
toc: style_simple
cover: 'https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/73.JPG'
description: 前缀和
abbrlink: aac8ec44
date: 2022-08-28 17:08:44
---



# [题目大意](http://oj.daimayuan.top/course/10/problem/883)

![题意](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220824011812015.png)

# 分析

由 $(a,b,c) = (a,a+b,a+b+c)$

对于 $f(S)$,我们可以考虑其前缀和数组 $sum[i]$

则  $f(S) = f(sum)$

对于前缀和操作 $1,2$ 相对于对 $sum[i] \  +1  \ or -1$，且 $sum[n]$ 无法被修改

我们令 $f(sum) = x$, 显然 $x$ 一定是 $sum[n]$ 的一个因子

这样我们可以枚举 $sum[n]$ 的因子 $d$

来判断 $sum[i] \  \%  \ d = 0,i∈[i,n]$ 

对于最小操作次数 $res$

若 $sum[i] \  \%  \ d = 0$，则不贡献 $res$

否则， 会贡献 $res = min( sum[i]\%d,d - sum[i]\%d)$

$sum[i]\%d$ 贡献的就是 $+1$ 操作，反之 $-1$ 操作

# Code

```cpp
signed main()
{
    int n;cin>>n;
    vector<int> a(n+1);
    int res = 0;
    int sum = 0;
    for(int i = 1;i <= n;i++) cin >> a[i],sum += a[i];
    if(sum == 1) puts("-1");
    else{
        vector<int> d;
        int v = sum;
        for(int i = 2;i <= v/i;i++){
            if(v % i == 0){
                d.push_back(i);
                while(v % i == 0)  v /= i;
            }
        }
        if(v > 1) d.push_back(v);
        int res = inf;
        for(auto p:d){
            int pre = 0;
            int now = 0;
            for(int i = 1;i <= n;i++){
                pre += a[i];
                if(pre % p){
                    now += min(pre%p,p - pre%p);
                }
            }
            res = min(res,now);
        }
        cout << res << endl;
    }
    return 0;
}
```

