---
title: LCM与GCD
tags:
  - 每日一题
categories:
  - 算法
toc: style_simple
cover: 'https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/55.jpg'
description: 数学推导+质因数分解优化
abbrlink: 23b5ec49
date: 2022-08-20 02:47:40
mathjax: true
---

# [题目大意](http://oj.daimayuan.top/course/10/problem/953)

![题意](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220809053835895.png)



# 分析

令 

$g = gcd(a,b)$

$a = A*g$

$b = B*g$

则

$(A,B) = 1$ 

$lcm(a,b) = a*b/(a,b) = A*B*g$

带入式子得

$x*A*B*g-y*g = k$

$g (x*A*B-y) = k$

$g$  可以通过枚举 $k$ 的因子得到

令 $t=x*A*B-y$ ,则

若能确定$A,B$ ,则可以唯一确定 $a,b$,移项得

$A*B = (t+y) / x$ , 注意到 $(A,B) = 1$,说明 $A,B$ 中的质因子没有相同的

若 $(t+y)/x$ 的质因子种类数有 $cnt$ 个，则贡献答案 $2^{cnt}$(考虑质因子分解给 $A$ 或 $B$)

# Code



```cpp
int p[20000005];
int st[20000005];
int cnt = 0;
void get()
{
    st[1] = 1;
    for (int i = 2; i <= 20000004; i++)
    {
        if (!st[i])
        {
            p[++cnt] = i;
            st[i] = i;
        }
        for (int j = 1; j <= cnt && i * p[j] <= 20000005; j++)
        {
            if(!st[i * p[j]]){
                st[i * p[j]] = p[j];
            }
            if (i % p[j] == 0)
                break;
        }
    }
}


int x, y, k;

// 分解质因数的一个优化
int find(int x){
    int cnt = 0;
    while(x>1){
        int t = st[x];
        while(x % t == 0) x /= t;
        cnt++;
    }
    if(x > 1) cnt++;
    return cnt;
}

int cal(int t){
    if((t+y)%x) return 0;
    return (1ll <<(find((t+y)/x)));
}

signed main()
{
    get();
    int t;
    cin >> t;
    while (t--)
    {
        cin >> x >> y >> k;
        int res = 0;
        for (int i = 1; i <= k / i; i++)
        {
            if (k % i == 0){
                res += cal(i);
                if(i != k/i) res += cal(k/i);
            }
 
        }
        cout << res << endl;
    }
    return 0;
}
```



![质因数分解优化](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220809054604335.png)

