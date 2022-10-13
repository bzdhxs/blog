---
title: Black Hole
tags:
  - 每日一题
categories:
  - 算法
mathjax: true
toc: style_simple
cover: >-
  https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/%E6%8F%92%E7%94%BB17.png
abbrlink: 2fc4a628
date: 2022-08-01 01:35:55
description: 计算几何
---

## [题目大意](https://ac.nowcoder.com/acm/contest/33189/L)

​	求边长为$a$的凸正$n$面体收缩$k$次后的面数和边长。

​	一次收缩定义为作该几何体所有面中心的三维凸包。

​	

## 分析

​	![image-20220801013418638](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220801013418638.png)



## Code

```cpp
int n;
signed main()
{   
    cin>>n;
    while(n--){
        int n,k;
        double a;
        cin>>n>>a>>k;
        if(n!=4&&n!=6&&n!=8&&n!=12&&n!=20) {
            puts("impossible");
            continue;
        }
        cout <<"possible"<<" ";
        while(k--){
            if(n==4) n = 4,a = a/3;
            else if(n==6) n = 8, a=a/sqrt(2);
            else if(n==8) n = 6,a  = a*sqrt(2)/3.0;
            else if(n==12) n = 20,a= a*(3*sqrt(5.0)+5.0)/10.0;
            else if(n==20) n = 12,a = a*(sqrt(5)+1)/6.0;
        }
        printf("%lld %.12f\n",n,a);
    }
    return 0;
}
```



