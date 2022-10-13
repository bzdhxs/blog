---
title: New Stone Game
tags:
  - 每日一题
categories:
  - 算法
mathjax: true
toc: style_simple
cover: 'https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/84.JPG'
description: 博弈论
abbrlink: 88808f29
date: 2022-09-21 10:00:29
---

New Stone Game

# [题目大意](http://oj.daimayuan.top/course/10/problem/854)

![题意](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220920094053391.png)



# 分析

``考虑到每个玩家第一次移除石头时，必须将所选石堆的石子完全移除``

那么前两手必定是在``对角线``上

![分析](https://cdn.jsdelivr.net/gh/bzdhxs/picgodemo/img/image-20220920094445278.png)

且最终决定胜负前一手状态一定是如图 $1.2$ 所示

那么问题就是考虑一个 $NIM$ 游戏, 对除前两轮选取的数之外的数进行 $XOR$

如果不在所选两个数的对角线，$xor$ 本身，其余 $xor$ 本身 - 1

NIM_SUM  为  1 ， 先手必胜，否则先手必败

那么可以枚举 $Alice$ 的位置，然后再枚举不和 $Alice$ 同行或同列 $Bob$ 的位置

再进行求 NIM_SUM,判断即可

# Code



```cpp
int a[3][3];
void solve(){
    int cot = 0;
    for(int i = 0;i < 3;i++) for(int j = 0;j < 3;j++) cin >> a[i][j];
    int res = 0;
    for(int i = 0;i < 3;i++)
        for(int j = 0;j < 3;j++){
            int f = 0;
            for(int ii = 0;ii < 3;ii++){
                for(int jj = 0;jj < 3;jj++){
                    int sum = 0;
                    if(ii == i || jj == j) continue;
                    for(int l = 0;l < 3;l++){
                        for(int r = 0;r < 3;r++){
                            if(l == i && r == j) continue;
                            if(l == ii && r == jj) continue;
                            if(l == i || l == ii || r == j || r== jj) {
                                sum ^= (a[l][r] - 1);
                            }
                            else sum ^= a[l][r];
                        }
                    }
                    if(!sum) f = 1;
                }
            }
            if(!f) res++; 
        }
    cout << res << endl;
}
```

