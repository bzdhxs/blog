---
title: Task Computing
tags:
  - 每日一题
categories:
  - 算法
mathjax: ture
description: sort,dp

toc: style_simple
cover: >-
  https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/%E6%8F%92%E7%94%BB13.png
abbrlink: e653c80a
date: 2022-07-31 22:12:01
---
## [题目大意](https://ac.nowcoder.com/acm/contest/33189/A)

​	从n个任务中选 $m$ 个$(a1,a2,,,am)$出来并任意排序，收益是  $\sum_{i=1}^mw_{a_i}\prod_{j}^{i-1}p_{a_j}$，求最大收益



## 分析

考虑一种排序方案使得按此方案可以计算出最大收益

考虑  $a_x$ 放在  $a_y$ 前收益更高的条件

![分析](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/QQ%E6%88%AA%E5%9B%BE20220731220639.jpg)



按照  $w_x - w_x*p_y > w_y - w_y*p_x$  排序 ，这样 x 在 y 前一定更优 

$m <=20$，做背包

$f[i][j]$ 为 考虑 $[i \ -  n]$ ,取了 $j$ 个 的最大价值

注意为何``从后往前``



## Code

```cpp
int n,m;
struct node{
    int w;
    double q;
    bool operator<(const node&t) const{
        return w-w*t.q > t.w - t.w * q;
    }
}a[100005];

double f[100005][21];

signed main()
{
    cin>>n>>m;

    for(int i = 1;i <= n;i++) cin >> a[i].w;
    for(int i = 1;i <= n;i++) cin >> a[i].q,a[i].q /= 10000.0;

    sort(a+1,a+1+n);
    for(int i = n;i>=1;i--)
        for(int j = 1;j <= m;j++){
            f[i][j] = max(f[i+1][j],f[i+1][j-1]*a[i].q+a[i].w);
        }
    printf("%.6f\n",f[1][m]);
    return 0;
}
```

