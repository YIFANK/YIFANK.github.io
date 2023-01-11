## SP34096 DIVCNTK 题解

题意：求$S_k(n)=\sum_{i=1}^{n}d(i^k)$。

数据范围：多组数据，$1\le T\le 10^4,n,k\le 10^{10}$

思路：设$f(x)=d(x^k)$，发现$f(p^c)=kc+1$，在质数幂时可以快速求值，于是我们可以用$Min\_25$筛。

时间复杂度：$O(\dfrac{n^{0.75}}{\log n}+n^{1-\epsilon})$

代码：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef unsigned long long ll;
const int N=1e6+5;
int T,sqr;
ll n,k,lmt,num,pri[N],val[N<<1],tot,g[N<<1],id1[N],id2[N];
bool v[N];
inline ll S(ll x,int y){
    if(x<pri[y])return 0;
    ll t=(x<=sqr?id1[x]:id2[n/x]);
    ll ans=g[t]-y*(k+1);
    for(int i=y+1;i<=num&&1ll*pri[i]*pri[i]<=x;i++){
        ll pe=pri[i];
        for(int e=1;pe<=x;e++,pe=pe*pri[i]){
            ans+=(k*e+1)*(S(x/pe,i)+(e!=1));
        }
    }
    return ans;
}
int main(){
    for(int i=2;i<N;i++){
        if(!v[i])pri[++num]=i;
        for(int j=1;j<=num&&pri[j]*i<N;j++){
            v[i*pri[j]]=1;
            if(i%pri[j]==0)break;
        }
    }
    scanf("%d",&T);
    while(T--){
        scanf("%lld%lld",&n,&k);
        sqr=sqrt(n);
        for(lmt=1;lmt<=num&&pri[lmt]<=sqr;)lmt++;lmt--;
        tot=0;
        for(ll l=1,r;l<=n;l=r+1){
            r=n/(n/l);
            val[++tot]=n/l;
            g[tot]=val[tot]-1;
            if(n/l<=sqr)id1[n/l]=tot;
            else id2[r]=tot;
        }
        for(int j=1;j<=lmt;j++){
            for(int i=1;pri[j]*pri[j]<=val[i];i++){
                ll k=val[i]/pri[j];
                k=(k<=sqr?id1[k]:id2[n/k]);
                g[i]-=(g[k]-j+1);
            }
        }
        for(int i=1;i<=tot;i++)g[i]*=(k+1);
        printf("%llu\n",S(n,0)+1);
    }
    return 0;
}
```

