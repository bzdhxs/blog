---
title: 'Codeforces Round #804 (Div. 2) vp'
description: vp of 1699
tag:
  - codeforces
categories:
  - 算法
cover: https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/%E6%8F%92%E7%94%BB21.jpg
date: '2022-07-12 15:12'
mathjax: true
toc: style_simple
abbrlink: 40fe3d8c
---



## A [The Third Three Number Problem](https://codeforces.com/contest/1699/problem/A)

考虑特值

n为奇无解

n为偶数， $a,b,c = [0,0,n/2]$;

### Code

```cpp
void solve(){
    int n;
    cin>>n;
    if(n&1) cout << -1 << endl;
    else {
        cout << 0 <<" "<< 0 <<" "<< n/2 << endl;
    }
}
```



## B [Almost Ternary Matrix](https://codeforces.com/contest/1699/problem/B)

观察可知

$res$ 的子矩阵为:

$$
\begin{matrix} 	
    1&0&0&1\\ 
    0&1&1&0\\ 
    0&1&1&0\\ 
    1&0&0&1
\end{matrix}
$$

### Code

```cpp
int n,m;
string s[51];
void init(){
    forr(i,0,49){
        int f = 0;
        forr(j,1,25){
            if(i%4==0 || i%4==3){
                if(!f) s[i] += "10";
                else s[i] += "01";
                f ^= 1;
            }
            else{
                if(!f) s[i] += "01";
                else s[i] += "10";
                f ^= 1;
            }
        }
    }
}
void solve(){
    cin>>n>>m;
    forr(i,1,n){
        forr(j,1,m){
            if(s[i-1][j-1] == '0') cout << 0 <<" ";
            else cout << 1 <<" ";
        }
        cout << endl;
    }
}
```



## C [The Third Problem](https://codeforces.com/contest/1699/problem/C)

对于区间 $[l,r]$  $MEX([a_l,,,a_r]) = x$ 说明 $[0,x-1]$ 均在区间$[l,r]$内

所以枚举 $i$ ∈ [0,n-1]，并维护 $[0,i-1]$ 能到达的最左和最右端点(L,R)

枚举 $i$ 的位置 $p[i]$，若 $p[i]$ 在 $(L,R)$ 内,那么 $i$ 可以放在剩下的位置 $(len - i)$  

 $len = R - L + 1$，区间 $[L,R]$已经放了$[0,i-1]$i个数,

对 $res$ 的贡献为 $res \ *= (R-L+1-i)$

 

### Code

```cpp
void solve(){
    int n;cin>>n;
    vector<int> a(n+1),p(n+1);
    forr(i,1,n) cin >> a[i],p[a[i]] = i;
    int l = inf,r = -inf;
    int res = 1;
    forr(i,0,n-1){
        if(l <= p[i] && p[i] <= r){
            res *= (r-l+1-i);
            res %= mod;
        }
        l = min(l,p[i]);
        r = max(r,p[i]);
    }
    cout << res << endl;
}
```





## D [Almost Triple Deletions](https://codeforces.com/contest/1699/problem/D)

一段连续的数可以完全删完需要满足

1. 长度为偶数
2. 众数个数不大于长度一半

根据  $n <= 5000$ ,可以 $n^2$ 的处理出所有区间能否转移的情况

考虑 dp[i]  表示以第i位结尾并且作为答案贡献的最大个数

转移方程

$dp[i] = max(dp[i],dp[j]+1) \ a[i] == a[j] \&\& ok(i+1,j-1)$

要预处理 $dp$ 当 $ok(1,i-1) \ dp[i] = 1$ 表示 $i$ 以前的数可以完全删除



### Code

```cpp
int f[5005][5005];
bool ok(int l,int r){
    if(r-l+1 <= 0) return 1;
    return (r-l+1)%2==0 && f[l][r];
}
void solve(){
    int n;cin>>n;
    vector<int> a(n+1);
    forr(i,1,n) cin >> a[i];
    forr(i,1,n){
        vector<int> cot(n+1,0);
        int mx = -inf;
        forr(j,i,n){
            ++cot[a[j]];
            mx = max(mx,cot[a[j]]);
            f[i][j] = (2*mx<=(j-i+1));
        }
    }
    vector<int> dp(n+5,-inf);
    forr(i,1,n) if(ok(1,i-1)) dp[i] = 1;
    forr(i,1,n){
        for(int j = i-1;j>=1;j--){
            if(ok(j+1,i-1) && a[i] == a[j])
                dp[i] = max(dp[i],dp[j]+1);
        }
    }
    int res = 0;
    forr(i,1,n) if(ok(i+1,n)) res = max(res,dp[i]);
    cout << res << '\n';
}
```

