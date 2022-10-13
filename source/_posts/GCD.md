---
title: GCD
tags:
  - 每日一题
categories:
  - 算法
mathjax: true
toc: style_simple
cover: 'https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/63.png'
description: 区间互质 && 容斥原理
abbrlink: 2071db18
date: 2022-08-20 03:27:57
---

# [题目大意](http://oj.daimayuan.top/course/10/problem/871)

![题意](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220820031916361.png)

![题意](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220820031933928.png)

# 分析

令

 $ t = gcd(a,m) $

$t = gcd(a+x,m) \Longrightarrow gcd((a+x)/t,m/t) = 1$

$x ∈ [0,m)$ 

令 $m = m/t  ,  k = (a+x)/t ， k∈[(a+t-1)/t,(m+a-1)/t]$ 

则问题转化为求区间 $k$ 与$m$ 互质的个数

区间互质问题可以考虑容斥原理

# Code

```cpp
namespace RC{
    typedef long long LL;
    int T, N, num;
    LL A, B;
    int a[100];
    void prime(int n) {
        num = 0;
        for (int i = 2; i*i <= n; i++) {
            if ((n%i) == 0) {
                num++;
                a[num] = i;
                while ((n%i) == 0) {
                    n /= i;
                }
            }
        }
        if (n > 1) {
            num++;
            a[num] = n;
        }
        return;
    }
    // 容斥原理 求 1-r 不被n的质因子整除的个数
    LL solve(LL r, int n) {
        prime(n);
        LL res = 0;
        for (int i = 1; i < (1 << num); i++) {
            int kk = 0;
            LL div = 1;
            for (int j = 1; j <= num; j++) {
                if (i&(1 << (j - 1))) {
                    kk++;
                    div *= a[j];
                }
            }
            if (kk % 2)
                res += r / div;
            else
                res -= r / div;
        }
        return r - res;

    }

    LL que(LL L, LL R, LL k) {
        return solve(R, k) - solve(L - 1, k);
    }
}

int gcd(int a,int b){
    return b?gcd(b,a%b):a;
}
void solve(){
    int a,m;cin>>a>>m;
    int t = gcd(a,m);
    int l = (a+t-1)/t;
    int r = (m+a-1)/t;
    m = m/t;
    cout << RC::que(l,r,m) << endl;
}

signed main()
{
    int t;cin>>t;
    while(t--) solve();
    return 0;
}
```

