# JOI 2021 Final

### T1. とてもたのしい家庭菜園 4

题意：

给一个长度为 $N$ 的数组 $A_i$ ，每次操作可以将任意一段区间 $[l,r]$ 中的数 $+1$ 。要求若干次操作后得到的序列 $B_i$ 满足存在一个 $k\in[1,N]$，使得 $[1,k]$ 的数严格递增，$[k+1,N]$ 的数严格递减。最小化操作次数。

解析：

只有区间操作，并且最后得到的数组满足一些单调性，我们可以想到用差分优化。这样每一个操作就变成将差分数组 $d$ 中的 $d_l$ 加一，$d_{r+1}$ 减一。最后的序列满足一段 $d_i>0$ 接一段 $d_i<0$ ，可以用前缀和算出令一段区间的 $d_i$ 满足条件的操作次数，后面枚举断点 $k$ 即可。

时间复杂度：$O(N)$

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define fi first
#define se second
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2e5+5;
int n,a[N],b[N];
ll f[N],g[N];
int main(){
    n=read();
    rep(i,1,n)a[i]=read();
    rep(i,1,n)b[i]=a[i]-a[i-1];
    rep(i,2,n){
        if(b[i]<=0){
            f[i]=f[i-1]-b[i]+1;
        }else f[i]=f[i-1];
    }
    per(i,n,2){
        if(b[i]>=0){
            g[i]=g[i+1]+b[i]+1;
        }else g[i]=g[i+1];
    }
    ll ans=1e18;
    rep(i,1,n)ans=min(ans,max(f[i],g[i+1]));
    printf("%lld\n",ans);
    return 0;
}
```



### T2. 雪玉

题意：

一个数轴上有 $N$ 个质量为 $0$ 的雪球，最初第 $i$ 个雪球的坐标为 $X_i$ ，数轴上覆盖了雪。有 $Q$ 天的强风，第 $j$ 天的风会使所有雪球同时移动 $W_j$ 距离。 如果有雪球滚过一个被雪覆盖的区间 $[a,a+1]$ ，区间上的雪会消失，同时雪球的质量增加 $1$ 。计算在 $Q$ 天后每个雪球的质量。

解析：

所有雪球一起平移，所以任意一个雪球都无法跨越相邻的雪球去积雪，而对一个雪球贡献雪的区间必然连续。于是我们考虑对于每一个雪球，计算它滚到雪的最左和最右端点 $l_i,r_i$ 。

具体地，我们发现两个雪球间的距离越短，里面的雪会先被滚完，可以把这些区间长度从小到大排序。记录向左和向右的最大位移 $L,R$ ，若当前的风力更新了这个位移，使得 $L+R$ 超过了某个区间的大小，就可以维护 $r_i,l_{i+1}$ ，即两点在区间内分雪的位置。

时间复杂度：$O(N\log N+Q)$ 

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<ll,int>
#define fi first
#define se second
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2e5+5;
ll n,q,a[N];
ll l[N],r[N],w,x,L,R;
pii d[N];
int main(){
    n=read(),q=read();
    rep(i,1,n){
        a[i]=read();
        l[i]=r[i]=-1;
    }
    rep(i,1,n-1)d[i]={a[i+1]-a[i],i};
    sort(d+1,d+n);
    int now=1;
    while(q--){
        x=read();w+=x;
        if(w>0)R=max(R,w);
        else L=max(L,-w);
        while(L+R>=d[now].fi&&now<n){
            int i=d[now].se;
            if(w>0)r[i]=a[i+1]-L;
            else r[i]=a[i]+R;
            l[i+1]=r[i];
            ++now;
        }
    }
    rep(i,1,n){
        if(l[i]==-1)l[i]=a[i]-L;
        if(r[i]==-1)r[i]=a[i]+R;
    }
    rep(i,1,n)printf("%lld\n",r[i]-l[i]);
    return 0;
}
```

### T3. 集合写真

题意：

给一个 $1\sim N$ 的排列 $a$ ，要求经过若干次交换 $a_i,a_{i+1}$ 后，对所有 $i$ 均满足 $a_i<a_{i+1}+2$ 。求最少的交换次数。

解析：

刚开始很没有头绪，不妨看看操作完后的序列长什么样子。由 $a_i<a_{i+1}+2$ 得 $a_i\le a_{i+1}+1$ ，也就是一个数最多比后面的数大 $1$ 。那么可以把最终的序列分成若干个下降序列，即分为 $k$ 个区间 $[l_1,r_1],[l_2,r_2],\dots,[l_k,r_k]$ 。不难发现 $[l_i,r_i]$ 中的数只能是 $r_i \sim l_i$ 。

考虑对于这样的合法序列最小操作次数是什么，设原排列中 $i$ 在 $p_i$ 位置，则新序列到原序列的操作次数和逆序对有关，即：
$$
\sum_{i=1}^n\sum_{j=i+1}^n[p_{a_i}>p_{a_j}]
$$
设 $f_i$ 为将前 $i$ 个数操作成合法序列的最小代价，枚举最后一个区间的起点可得 $$f_i=\min_{1\le j\le i}\{f_{j-1}+\operatorname{calc}(j,i)\}$$ ，接下来只用考虑如何快速计算 $\operatorname{calc}$ 函数。

对于 $k\in[j,i]$ ，我们计算它与 $[1,j-1]$ 中的逆序对数和它与自己区间内的逆序对数。知道在新序列中 $x<j,k<x\le i$ 在它前面，$j\le x<k$ 在它后面。倒序枚举 $j$ ，逆序对数可以开两个树状数组优化。

时间复杂度：$O(N^2\log N)$

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define fi first
#define se second
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2e5+5,inf=1e9;
int n,p[N],f[N],A[N],B[N];
void I(int *t,int x,int val){
    for(;x<=n;x+=x&-x)t[x]+=val;
}
int Q(int *t,int x){
    int res=0;
    for(;x;x-=x&-x)res+=t[x];
    return res;
}
int main(){
    n=read();
    rep(i,1,n){
        int x=read();
        p[x]=i;
    }
    rep(i,1,n){
        //printf("i=%d:\n",i);
        f[i]=inf;
        int sum=0;
        memset(B,0,sizeof(B));
        I(A,p[i],1);
        per(j,i,1){
            sum+=i-Q(A,p[j])-Q(B,p[j]);
            //printf("j=%d: sum=%d,f=%d\n",j,sum,f[j-1]);
            f[i]=min(f[i],f[j-1]+sum);
            I(B,p[j],1);
        }
    }
    printf("%d\n",f[n]);
    return 0;
}
```

###  T4. ロボット

题意：

给定一个 $N$ 个点，$M$ 条边的无向图，第 $i$ 条边的颜色为 $C_i\in[1,M]$ ，但不保证 $C_i$ 互不相等。

有一个智能机器人，你告诉这个机器人一个颜色，如果它当前所在节点有且仅有一条这种颜色的边，它就会走这条路，否则它会停留在原地。

现在这个机器人从点 $1$ 出发想移动到点 $N$ ，但是你不一定能通过仅仅告诉它颜色来使它走到 $N$ 。因此你可以**预先**改变一些边的颜色，改变第 $i$ 条边的代价是 $P_i$ 。求最小代价，如无解则输出 $-1$ 。

解析：

如果当前边唯一，我们可以直接走。否则如果我们想走一条边，可选的操作只有两种：

1. 把它的颜色换成新颜色。

2. 改变所有和它颜色相同的边的颜色。

发现好像并不能这样建图跑 dijkstra，因为有特殊情况:

若有 $(u,v),(v,w)$ 两边颜色相同，而我们对 $(u,v)$ 使用了操作 1，对 $(v,w)$ 使用了操作 2，则会重复计算对 $(u,v)$ 操作的代价。考虑建虚点来去掉这个代价，对每条边 $(u,v,c)$ 建 $u$ 向 $v_c$ 的边权为 $0$ 的有向边，即省去了操作 1 的代价。$v_c$ 向 $w$ 连边权为操作 2 代价的有向边，这样 $u\to v_c\to w$ 相当于进行了一次操作 1 和一次操作 2。

注意到我们对每条边都建一个虚点，这样的点数是 $O(N+M)$ 的，边数是 $O(M)$ 的，可以用 dijkstra 通过。

时间复杂度：$O((N+M)\log(N+M))$

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define pii pair<int,int>
#define fi first
#define se second
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
struct node{
    ll d;int u,c;
    friend bool operator <(node a,node b){
        return a.d>b.d;
    }
};
priority_queue <node> Q;
const int N=1e5+5,M=4e5+5;
int n,m,tot,head[N];
struct E{
    int to,nxt,c,w;
}e[M];
map <int,ll> sum[N],dis[N];
map <int,vector<pii>> mp[N];
inline void addedge(int u,int v,int c,int w){
    e[++tot]={v,head[u],c,w};head[u]=tot;
    mp[u][c].push_back({v,w});
    sum[u][c]+=w;
}
inline void upd(int u,int c,ll d){
    if(!dis[u].count(c)||dis[u][c]>d){
        dis[u][c]=d,Q.push({d,u,c});
    }
}
inline void dijkstra(){
    upd(1,0,0);
    while(!Q.empty()){
        node tmp=Q.top();Q.pop();
        ll d=tmp.d;
        int u=tmp.u,c=tmp.c;
        if(dis[u][c]!=d)continue;
        if(!c){
            for(int i=head[u];i;i=e[i].nxt){
                int v=e[i].to;
                upd(v,e[i].c,d);
                upd(v,0,d+min((ll)e[i].w,sum[u][e[i].c]-e[i].w));
            }
        }else{
            if(dis[u].count(c)){
                for(pii tmp:mp[u][c]){
                    int v=tmp.fi,w=tmp.se;
                    upd(v,0,d+sum[u][c]-w);
                }
            }
        }
    }
}
int main(){
    n=read(),m=read();
    rep(i,1,m){
        int u=read(),v=read(),c=read(),w=read();
        addedge(u,v,c,w),addedge(v,u,c,w);
    }
    dijkstra();
    if(dis[n].count(0))printf("%lld\n",dis[n][0]);
    else puts("-1");
    return 0;
}
```

### T5. ダンジョン 3

题意：

有一个 $N+1$ 层的地牢和 $M$ 个人。玩家只能从一层移动到下一层，对于 $1\le i\le N$ ，从第 $i$ 层移动到第 $i+1$ 层需要能量 $A_i$ 。每层还有治疗泉，在第 $i$ 层的治疗泉中，玩家可以花 $B_i$ 金币来购买一格能量。但是，每个玩家都有一个能量上限，使用治疗泉无法使能量超过该上限。

现在告诉你第 $j$ 个玩家在第 $S_j$ 层，要去 $T_j$ 层且他初始能量为 $0$ ，能量上限为 $U_j$ 。在路上他的能量不能小于 $0$ ，那么他需要多少金币呢？

解析：

先把每一层看作数轴上的点，即第一层看作数轴原点，两层之间距离为 $A_i$ 。不难想到一个贪心：每次去最近的 $B$ 比当前点小（回复泉便宜）的点，并将能量回复到恰好到达这个点，否则就回满能量。

接下来先看子任务 $2$ ，即对所有人 $U$ 为定值的情况。我们可以用单调栈从后往前扫一遍做出对于第 $i$ 个点最优的转移点。这样可以做到 $O(N\log N)$ 。

再看子任务 $3$ ，$T_i=N+1$ 即所有人的终点相同，显然可以对起点排序后倒着扫一遍处理答案。注意到仍然可以用单调栈维护价格，如果当前的价格更便宜，就弹出栈顶并且考虑是否能在当前点买能量。

答案显然随着 $U$ 增大而减小，且可以把在 $i$ 点回复看作斜率为 $B_i$ 的一次函数，则本质上为若干个分段函数的和。离散化 $U$ 即可用树状数组维护斜率和截距。

最后，如果找出 $[S,T]$ 中离 $T$ 距离 $\le U$ 的最小位置 $mid$ ，设 $f(S,T)$ 为 $S$ 到 $T$ 的最小代价，有：
$$
f(S,T)=f(S,N+1)-f(T,N+1)-(T-mid)\times B_{mid}
$$
这样就解决了原题。

时间复杂度：$O(N\log N)$

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define fi first
#define se second
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2e5+5;
int n,m,a[N],b[N];
int lg[N],lsh[N],top,stk[N],tp;
ll x[N],ans[N];
pii mx[N][20],mn[N][20];
struct BIT{
    ll c1[N],c2[N];
    void add(ll x,ll y,ll z){
        int i=lower_bound(lsh+1,lsh+top+1,x)-lsh;
        for(;i<=n;i+=i&-i)c1[i]+=y,c2[i]+=z;
    }
    ll sum(ll x){
        ll res1=0,res2=0;
        int i=lower_bound(lsh+1,lsh+top+1,x)-lsh;
        for(;i;i-=i&-i)res1+=c1[i],res2+=c2[i];
        return res1*x+res2;
    }
}T;
struct query{int u,id,t;};
vector <query> Q[N];
inline pii Mx(int l,int r){
    int L=lg[r-l+1];
    return max(mx[l][L],mx[r-(1<<L)+1][L]);
}
inline pii Mn(int l,int r){
    int L=lg[r-l+1];
    return min(mn[l][L],mn[r-(1<<L)+1][L]);
}
int main(){
    //freopen("02-01.in","r",stdin);
    //freopen("tes.out","w",stdout);
    n=read(),m=read();
    lg[0]=-1;
    rep(i,1,n){
        a[i]=read(),mx[i][0]={a[i],i};
        x[i]=x[i-1]+a[i];
        lg[i]=lg[i>>1]+1;
    }
    per(i,n+1,1)x[i]=x[i-1];
    rep(i,1,n){
        b[i]=read(),mn[i][0]={b[i],i};
    }
    rep(j,1,20)rep(i,1,n-(1<<j)+1){
        mx[i][j]=max(mx[i][j-1],mx[i+(1<<(j-1))][j-1]);
        mn[i][j]=min(mn[i][j-1],mn[i+(1<<(j-1))][j-1]);
    }
    rep(i,1,m){
        int s=read(),t=read(),u=read();
        if(Mx(s,t-1).fi>u){
            ans[i]=-1;continue;
        }
        ll l=max((ll)(lower_bound(x+1,x+n+1,x[t]-u)-x),(ll)s);
        int mid=Mn(l,t-1).se;
        assert(mid<t);
        ans[i]=(x[t]-x[mid])*b[mid];
        Q[s].push_back({u,i,1}),Q[mid].push_back({u,i,-1});
        lsh[++top]=u;
    }
    sort(lsh+1,lsh+top+1);
    top=unique(lsh+1,lsh+top+1)-lsh-1;
    stk[0]=n+1;
    per(i,n,1){
        while(b[stk[tp]]>b[i]){
            int u=stk[tp],v=stk[tp-1];tp--;
            ll L0=x[u]-x[i],L1=x[v]-x[i];
            T.add(L0,-b[u],L0*b[u]);
            T.add(L1,b[u],-L1*b[u]);
        }
        ll L0=x[stk[tp]]-x[i];
        T.add(0,b[i],0);
        T.add(L0,-b[i],L0*b[i]);
        stk[++tp]=i;
        for(auto q:Q[i])ans[q.id]+=1ll*q.t*T.sum(q.u);
    }
    rep(i,1,m)printf("%lld\n",ans[i]);
    return 0;
}
```



