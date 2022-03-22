---
layout: post
title:  "20220320校内考试 T2 string 题解"
categories: 题解
tags: 洛谷
author: ex_Asbable
---

* content
{:toc}

## 20220320校内考试 T2 string 题解

### 题目描述

给出 $n$ 以及 $n$ 个单词（仅包含小写字母）形成一个句子，现打乱这个句子，但是对于任意单词，在原句子和乱序句子中第 i 次 出现的位置不会相差超过1。

求有多少个这样的乱序句子，对1000000007取模。

$n\le100000$

### 解法

首先，由“在原句子和乱序句子中第 i 次 出现的位置不会相差超过1”中，就可以知道，只用在原序列中判断是否交换相邻两数就好了。

既然如此，发现前 i 个单词的合法乱序个数不会受到后面单词的影响，故无后效性。

所以自然而然想到 dp 。

怎么设计状态？

很简单，设 dp\[i\] 表示前 i 个单词的合法乱序句子个数对1000000007取模的值。

故若当前单词与上一个单词不同的话（交换会使序列改变），则dp\[i\]=dp\[i-1\]+dp\[i-2\]

反之，若相同，则dp\[i\]=dp\[i-1\]（因为此时交换不影响结果，不需更新）

总结一下：

dp\[i\]=dp\[i-1\]+dp\[i-2\]（a\[i\]!=a\[i-1\]）

dp\[i\]=dp\[i-1\]（a\[i\]==a\[i-1\]）

别忘了取模

### CODE

```cpp
const ll mn=1e5+10;
const ll mm=1e5+10;
const ll mod=1000000007;
const ll inf=0x3f3f3f3f;
const ll fni=0xc0c0c0c0;
ll n,dp[mn];string s[mn];
inline void read_init(){
    n=read<ll>();
    for(ll i=1;i<=n;i++)
        cin>>s[i];
}
int main(){
//    freopen("string17.in","r",stdin);
//     freopen("string.out","w",stdout);
    read_init();
    dp[0]=1;
    for(ll i=1;i<=n;i++)
        dp[i]=(dp[i-1]+(s[i]==s[i-1]?0:dp[i-2]))%mod;
    write(dp[n]%mod,1);
    return 0;
}
```

