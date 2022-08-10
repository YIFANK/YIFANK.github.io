**题目大意：**

给定一个 $N$ 个点 $M$ 条边的森林，边有长度。假设图中有 $K$ 个树，我们希望从每个树中选取一条路径，并用 $K$ 条长度为 $X$ 的边将其连成环。问能组成多少种长度至少为 $Y$ 的环。

数据范围：$1\le N\le 1500,1\le M\le N-1,0\le X,Y\le 2500$ .

> 知识储备：树形dp

> 题目难度：省选/USACO Platinum

**解析：**

发现可以对每棵树 $O(n^2)$ dfs 来统计树上的路径长度以及该长度的数量。合并每颗树上的答案类似卷积，也可以 $O(K^2)$ 求出。具体地，设 $f_{i,0/1}$ 为总长度为 $i$ 的方案数/长度和，$g_{i,0/1}$ 为树上的长度为 $i$ 的路径数/长度和，有：
$$
f_{i+j,0}\leftarrow f_{i,0}\cdot g_{j,0}\\
f_{i+j,1}\leftarrow f_{i,0}\cdot g_{j,1}+f_{i,1}\cdot g_{j,0}\\
$$
 暴力转移即可，考虑这样暴力的时间复杂度为什么可以通过。首先我们只关注总长度是否超过 $Y$ ，所以可以将所有长度 $\ge Y$ 的记在一起。并且每颗树上最多 $n^2$ 个不同长度的路径，设第 $i$ 颗树的大小为 $n_i$ 。总时间复杂度则为 $O(\sum n_i^2+Ymin(Y,n_i^2))=O(NY\sqrt Y)$ ，当 $n_1=n_2=\cdots=\sqrt Y$ 时取最大值。

时间复杂度：$O(NY\sqrt Y)$ .

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define fi first
#define se second
#define pb push_back
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2505,P=1e9+7;
int ecnt,head[N];
struct Edge{
    int to,nxt,w;
}e[N<<1];
inline void add(int u,int v,int w){
    e[++ecnt]={v,head[u],w};head[u]=ecnt;
}
int n,m,X,Y,cnt,fa[N],col[N];
ll ans,f[N][2],g[N][2],sum[N][N],tot[N][N];
int find(int x){return fa[x]==x?x:fa[x]=find(fa[x]);}
void color(int u,int f,int id){//对同一个连通块染色
    col[u]=id;
    for(int i=head[u];i;i=e[i].nxt){
        int v=e[i].to;
        if(v==f)continue;
        color(v,u,id);
    }
}
void dfs(int u,int f,int len){//单个树上路径统计
    if(f){
        int c=col[u],l=min(len,Y);
        sum[c][l]=(sum[c][l]+len)%P;
        ++tot[c][l];
    }
    for(int i=head[u];i;i=e[i].nxt){
        int v=e[i].to;
        if(v==f)continue;
        dfs(v,u,len+e[i].w);
    }
}
int main(){
    //freopen("mooriokart.in","r",stdin);
    //freopen("mooriokart.out","w",stdout);
    n=read(),m=read(),X=read(),Y=read();
    rep(i,1,n)fa[i]=i;
    rep(i,1,m){
        int u=read(),v=read(),w=read();
        add(u,v,w);add(v,u,w);
        fa[find(u)]=find(v);
    }
    rep(i,1,n)if(i==find(i))color(i,0,++cnt);
    rep(i,1,n)dfs(i,0,0);
    //dp(卷积合并答案)
    int cur=min(cnt*X,Y);
    f[cur][0]=1,f[cur][1]=X*cnt;
    rep(i,1,cnt){
        rep(j,cur,Y){
            g[j][0]=f[j][0],g[j][1]=f[j][1];
            f[j][0]=f[j][1]=0;
        }
        rep(j,0,Y){
            if(!tot[i][j])continue;
            rep(k,cur,Y){
                if(!g[k][0])continue;
                int l=min(j+k,Y);
                f[l][0]=(f[l][0]+tot[i][j]*g[k][0]%P)%P;
                f[l][1]=(f[l][1]+sum[i][j]*g[k][0]%P+tot[i][j]*g[k][1])%P;
            }
        }
    }
    ans=f[Y][1];
    rep(i,1,cnt-1)ans=ans*i%P;
    printf("%lld\n",ans*((P+1)/2)%P);
    return 0;
}
```

