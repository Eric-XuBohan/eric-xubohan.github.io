## 题解 洛谷 P4145 上帝造题的七分钟 2 / 花神游历各国

### $\color{red}\text{PART 1 题目大意}$

一个带根号的RMQ问题。

1.区间修改$[l,r]$，使每一个数都开平方

2.区间求和$[l,r]$

### $\color{red}\text{PART 2 解法}$

既然是RMQ问题，那么肯定~~树状数组线段树单调队列ST表齐上阵~~，可是——

这个根号就比较难缠啊

似乎普通的RMQ问题解法在此题中黔驴技穷。

可是仔细一想，$\forall a_i$至多只会被开方6次后变为1.

因为最大数$10^{12}$被开方时，会依次变成$10^6,10^3,31,5,2,1$，然后免疫开根。

所以就可以暴力单点修改了耶( •̀ ω •́ )y

不过还是没那么简单，因为要特判1，所以还是要用线段树。而且区间查询也不能暴力啊

线段树稍稍改一下，存2个值域，一个最大值，一个和。不用懒标记，特判最大值为1时，就证明这个区间不用再次修改了。查询就和原来线段树一样就可以了。

### $\color{red}\text{PART 3 CODE}$

```cpp
const int mn=1e5+10;
const int mm=1e5+10;
const int mod=1e9+7;
const int inf=0x3f3f3f3f;
const int fni=0xc0c0c0c0;
struct stree{
    ll l,r;
    ll sum,dat;
    #define l(x) tr[x].l
    #define r(x) tr[x].r
    #define sum(x) tr[x].sum
    #define dat(x) tr[x].dat
}tr[mn<<2];
#define lc(x) (x<<1)
#define rc(x) (x<<1|1)
ll n,m,a[mn];
void build(ll now,ll l,ll r){
    l(now)=l;r(now)=r;
    if(l==r){
        dat(now)=sum(now)=a[l];
        return;
    }
    ll mid=(l+r)>>1;
    build(lc(now),l,mid);
    build(rc(now),mid+1,r);
    sum(now)=sum(lc(now))+sum(rc(now));
    dat(now)=max(dat(lc(now)),dat(rc(now)));
}
void sqr(ll now,ll l,ll r){
    if(l(now)==r(now) && l(now)>=l && r(now)<=r){
        sum(now)=sqrt(sum(now));
        dat(now)=sqrt(dat(now));
        return;
    }
    ll mid=(l(now)+r(now))>>1;
    if(l<=mid && dat(lc(now))>1)
        sqr(lc(now),l,r);
    if(r>mid && dat(rc(now))>1)
        sqr(rc(now),l,r);
    sum(now)=sum(lc(now))+sum(rc(now));
    dat(now)=max(dat(lc(now)),dat(rc(now)));
}
ll ask(ll now,ll l,ll r){
    if(l(now)>=l && r(now)<=r)
        return sum(now);
    ll mid=(l(now)+r(now))>>1,ans=0;
    if(l<=mid)ans+=ask(lc(now),l,r);
    if(r>mid)ans+=ask(rc(now),l,r);
    return ans;
}
inline void read_init(){
    n=read<ll>();
    for(ll i=1;i<=n;i++)
        a[i]=read<ll>();
    m=read<ll>();
    build(1,1,n);
}
int main(){
    read_init();
    for(ll i=1;i<=m;i++){
        bool op=read<ll>();
        ll l=read<ll>(),r=read<ll>();
        if(l>r)swap(l,r);
        if(op)write(ask(1,l,r),1);
        else sqr(1,l,r);
    }
    return 0;
}
```
