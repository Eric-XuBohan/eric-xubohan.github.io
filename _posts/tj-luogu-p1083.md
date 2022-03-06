## 题解 洛谷P1083 \[NOIP2012 提高组\] 借教室

### $\color{red}\text{PART 1 题目大意}$

给出初始$n$个数，和$m$个区间修改。

对于每一个区间修改，若减去给定的数后，此区间存在负数，则输出$-1$和该修改的编号。

若$m$个修改均可满足，则输出一行$0$

### $\color{red}\text{PART 2 解法}$

~~线段树是肯定的~~

修改区间时，我们只需要知道其区间中是否存在负数，所以只需判断区间内最小的数就可以了。

所以线段树的工作就是在给定的初始值上，对于每一次修改操作，求区间最小值。

### $\color{red}\text{PART 3 CODE}$

```cpp
const int mn=1e6+10;
const int mm=1e6+10;
const int mod=1e9+7;
const int inf=0x3f3f3f3f;
const int fni=0xc0c0c0c0;
ll n,m,a[mn];
struct stree{
    ll l,r;
    ll add,sum;
    #define l(x) tr[x].l
    #define r(x) tr[x].r
    #define add(x) tr[x].add
    #define sum(x) tr[x].sum
    #define siz(x) (tr[x].r-tr[x].l+1)
}tr[mn<<2];
#define lc(x) (x<<1)
#define rc(x) (x<<1|1)
void build(ll now,ll l,ll r){
    l(now)=l;r(now)=r;
    if(l==r){sum(now)=a[l];return;}
    ll mid=(l+r)>>1;
    build(lc(now),l,mid);
    build(rc(now),mid+1,r);
    sum(now)=min(sum(lc(now)),sum(rc(now)));
}
inline void spread(ll now){
    if(!add(now))return;
    sum(lc(now))+=add(now);
    sum(rc(now))+=add(now);
    add(lc(now))+=add(now);
    add(rc(now))+=add(now);
    add(now)=0;
}
void plu(ll now,ll l,ll r,ll o){
    if(l<=l(now) && r(now)<=r){
        add(now)+=o;
        sum(now)+=o;
        return;
    }
    spread(now);
    ll mid=(l(now)+r(now))>>1;
    if(l<=mid)plu(lc(now),l,r,o);
    if(r>mid)plu(rc(now),l,r,o);
    sum(now)=min(sum(lc(now)),sum(rc(now)));
}
ll ask(ll now,ll l,ll r){
    if(l<=l(now) && r(now)<=r)return sum(now);
    spread(now);
    ll mid=(l(now)+r(now))>>1,minn=inf;
    if(l<=mid)minn=min(minn,ask(lc(now),l,r));
    if(r>mid)minn=min(minn,ask(rc(now),l,r));
    return minn;
}
inline void read_init(){
    n=read<ll>();m=read<ll>();
    for(ll i=1;i<=n;i++)
        a[i]=read<ll>();
    build(1,1,n);
}
int main(){
    read_init();
    for(ll i=1;i<=m;i++){
        ll a=read<ll>(),b=read<ll>(),c=read<ll>();
        plu(1,b,c,-a);
        if(ask(1,b,c)<0){
            puts("-1");
            write(i,1);
            return 0;
        }
    }
    puts("0");
    return 0;
}
```
