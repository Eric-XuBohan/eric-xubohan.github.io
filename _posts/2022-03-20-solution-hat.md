---
layout: post
title:  "20220320校内考试 T1 hat 题解"
categories: 题解
tags: 洛谷
author: ex_Asbable
---

* content
{:toc}

## 20220320校内考试 T1 hat 题解

### 题目描述

给出一个正偶数 $n$ ，有 $n$ 个整数 $a_i$ ，并定义数组 $b$ ，使 $b_i=$ 除 $a_i$ 外的所有数 $\in a$ 的**异或**和。

给出数组 $b$ ，求 $a$ 。

### 解法

由数组 $b$ 的定义可知，设 $B$ 为数组 $b$ 的异或和，所以 $B$ 就等于每一个 $a_i$ 的异或和（因为每个 $a_i$ 都异或了 $n-1$ 次，且 $n$ 是偶数，$a_i\oplus a_i=0$ ）。那么 $\forall b_i=B\oplus a_i$ ，所以 $A\oplus b_i=A\oplus (A\oplus a_i)$ ，由于异或满足结合律，则 $A\oplus b_i=a_i$ ，我们要求的 $a_i$ 它自己就冒出来了。（有点像小学数学的异或版）

所以只需要求出异或和 $B$，然后一个一个地异或输出就好了。

### CODE

```cpp
const int mn=2e5+10;
const int mm=2e5+10;
const int mod=1e9+7;
const ll inf=0x3f3f3f3f3f3f3f3f;
const ll fni=0xc0c0c0c0c0c0c0c0;
ll n,xor_sum,b[mn];
inline void read_init(){
    n=read<ll>();
    for(ll i=1;i<=n;i++)
        b[i]=read<ll>();
}
int main(){
    // freopen("hat.in","r",stdin);
    // freopen("hat.out","w",stdout);
    read_init();
    for(ll i=1;i<=n;i++)
        xor_sum^=b[i];
    for(ll i=1;i<=n;i++)
        write(xor_sum^b[i],2);
    puts("");
    return 0;
}
```

