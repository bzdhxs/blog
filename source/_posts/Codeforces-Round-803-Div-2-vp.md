---
title: "Codeforces Round #803 (Div. 2) vp"
description: vp of 1698
tag:
  - codeforces
categories:
  - 算法
cover: https://raw.githubusercontent.com/bzdhxs/picgodemo/main/img/%E6%8F%92%E7%94%BB16.png
abbrlink: 1c663129
date: 2022-06-30 08:15:10
mathjax: true
toc: style_simple
---


## A  [XOR Mixup](https://codeforces.com/contest/1698/problem/A)

长度为 1 的数列中,有一个数是其他数 $XOR$ 来, 问这个数是多少

随便输出数列中的一个数就行



### Code

```cpp
void solve(){
    int n;cin>>n;
    vector<int> a(n+1);
    int ans = 0;
    forr(i,1,n) cin >> a[i];
    cout << a[1] << endl;
}
```





## B [Rising Sand](https://codeforces.com/contest/1698/problem/B)



当 m = 1 时, 最大的数量就是 $(n-1)/2$;

m  > 1 时,最大数量就是原数列  $too\ tall$   的个数；

### Code

```cpp
void solve(){
    int n,m;
    cin>>n>>m;
    vector<int> a(n+1);
    forr(i,1,n) cin >> a[i];
    if(m == 1){
        int res = (n-1)/2;
        cout << res << endl;
    }
    else{
        int res = 0;
        forr(i,2,n-1){
            if(a[i] > a[i-1]+a[i+1]) res++;
        }
        cout << res << endl;
    }
}
```





## C [3SUM Closure](https://codeforces.com/contest/1698/problem/C)

当正数个数或负数个数 >= 3 时,必定不符合条件,输出 no；

当0的个数大于0时,最多有一正一负且``枚举1个0``即可,否则输出 no；

故,满足条件的数组元素个数最多是4个$O(n^3)$ 枚举即可



### Code

```cpp
void solve(){
    int n;
    cin>>n;
    map<int,int> mp;
    vector<int>v;
    int t = 0;
    int a =  0, b = 0;
    forr(i,1,n){
        int x;cin>>x;
        mp[x] = 1;
        if(x>0)a++,v.push_back(x);
        else if(x<0)b++,v.push_back(x);
        else t++;
    }
    if(t) v.push_back(0);
    if(a>=3||b>=3){
        puts("no");
        return;
    }
  forr(i,0,v.size()-1)
      forr(j,i+1,v.size()-1)
      	forr(k,j+1,v.size()-1){
        	int g = 0;
        	if(mp[v[i]+v[j]+v[k]]){
            	g = 1;
        	}
        	if(!g){
            	puts("no");
            	return ;
        	}
    	}
    puts("yes");
}
```





## D  [Fixed Point Guessing](https://codeforces.com/contest/1698/problem/D)

交互题

发现  $log^{1e4} ≈ 14$ 考虑二分答案

对于区间 $[l,r]$  内的数有两种情况

值在$[l,r]$范围内或者不在

值在区间[l,r]范围内的数也有两种情况

一种是与[l,r]内的值 $swap$,因而成对存在

另一种是在原位置

故对于[l,r]

若在这个范围内的数的个数为偶数,说明都是进行了 $swap$ 操作

那么答案一定不在这个区间里，二分搜另一个区间 


### Code

```cpp
vector<int> ask1(int l,int r){
    cout  << '?' <<" " << l << " " << r << endl;
    vector<int> v;
    forr(i,1,r-l+1){
        int x;
        cin>>x;
        v.push_back(x);
    }
    return v;
}

bool check(vector<int> x,int l, int r){
    int cnt = 0;
    for(auto i:x){
        if(i >=  l && i <= r) cnt++;
    }
    if(cnt%2==0) return 0;
    return 1;
}
void solve(){
    int n;
    cin >> n;
    int l = 1, r= n;
    int res = 0;
    while(l < r){
        int mid = l + r  >> 1;
        int f = 0;
        vector<int> b;
        b = ask1(l,mid);
        int t =  check(b,l,mid);
        if(t){
            r = mid;
        }
        else{
            l = mid + 1;
        }
        
    }
    cout << '!' <<" " << l << endl;
}
```

