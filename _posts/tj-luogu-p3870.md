## 题解 洛谷 \[P3870 \[TJOI2009\] 开关 \]

### $\color{red}\text{PART 1 题目大意}$

两种操作。

1.对于区间$[l,r]$，修改该区间状态(1变0，0变1)

2.对于区间$[l,r]$,查询该区间内1的个数

### $\color{red}\text{PART 2 解法}$

很明显，区间修改，区间查询，用线段树。

这是一个取反的线段树。很明显，取反后该区间内为1的个数=区间长度-原有1的个数。

对于懒标签，由于取反后再取反等同于没取反，故只需要异或即可。

### $\color{red}\text{PART 3 CODE}$

```cpp
const int mn=2e5+10;
const int mm=2e5+10;
const int mod=1e9+7;
const int inf=0x3f3f3f3f;
const int fni=0xc0c0c0c0;
int n,m;
struct stree{
    int l,r;
    int add,sum;
    #define l(x) tr[x].l
    #define r(x) tr[x].r
    #define add(x) tr[x].add
    #define sum(x) tr[x].sum
    #define siz(x) (tr[x].r-tr[x].l+1)
}tr[mn<<2];
#define lc(x) (x<<1)
#define rc(x) (x<<1|1)
void build(int now,int l,int r){
    l(now)=l;r(now)=r;
    if(l==r)return;
    int mid=(l+r)>>1;
    build(lc(now),l,mid);
    build(rc(now),mid+1,r);
}
inline void spread(int now){
    if(!add(now))return;
    sum(lc(now))=siz(lc(now))-sum(lc(now));
    sum(rc(now))=siz(rc(now))-sum(rc(now));
    add(lc(now))^=add(now);
    add(rc(now))^=add(now);
    add(now)=0;
}
void plu(int now,int l,int r){
    if(l<=l(now) && r(now)<=r){
        add(now)^=1;
        sum(now)=siz(now)-sum(now);
        return;
    }
    spread(now);
    int mid=(l(now)+r(now))>>1;
    if(l<=mid)plu(lc(now),l,r);
    if(r>mid)plu(rc(now),l,r);
    sum(now)=sum(lc(now))+sum(rc(now));
}
int ask(int now,int l,int r){
    if(l<=l(now) && r(now)<=r)return sum(now);
    spread(now);
    int mid=(l(now)+r(now))>>1,sum=0;
    if(l<=mid)sum+=ask(lc(now),l,r);
    if(r>mid)sum+=ask(rc(now),l,r);
    return sum;
}
inline void read_init(){
    n=read<int>();m=read<int>();
    build(1,1,n);
}
int main(){
    read_init();
    for(int i=1;i<=m;i++){
        int a=read<int>(),b=read<int>(),c=read<int>();
        if(!a)plu(1,b,c);
        else write(ask(1,b,c),1);
    }
    return 0;
}
```