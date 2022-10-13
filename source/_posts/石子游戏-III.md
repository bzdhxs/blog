---
title: 石子游戏 III
tags:
  - 每日一题
categories:
  - 算法
mathjax: true
toc: style_simple
cover: 'https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/86.JPG'
description: 博弈论
abbrlink: 6e1b1d3c
date: 2022-09-21 10:12:20
---



# [题目大意](http://oj.daimayuan.top/problem/845)

![题目大意](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220829150909512.png)





# 分析

结论

令 $t$ 为最小值

$if  \ t  > n/2 , then  \  Bob$

$else  \ Alice$

#	Code



```cpp
signed main()
{
    int n;cin>>n;
    vector<int> a(n+1);
    int sum = 0;
    for(int i = 1;i <= n;i++) cin >> a[i];
    int t = *min_element(a.begin()+1,a.end());
    int cnt = 0;
    for(int i = 1;i <= n;i++){
        if(a[i] == t) cnt++;
    }

    if(cnt > n/2) puts("Bob");
    else    puts("Alice");
    return 0;
}
```

