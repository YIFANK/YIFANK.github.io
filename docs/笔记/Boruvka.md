*（上场 Codeforces div.2 F题 没想出来，问了大佬 KarL05 才知道有除了 Kruskal 和 Prim 以外的一种生成树算法，故将其记录下来。）*

算法内容大概是维护每个连通块连出的最小边，每次迭代中对于每个连通块找到连出的最小边，找完后合并。由于原图连通，每次迭代中连通块个数至少减半，故算法迭代次数不超过 $O(\log V)$ 次。

算法详解：[Boruvka 算法](https://oiwiki.net/graph/mst/#boruvka)

### CF888G Xor-MST

**题目大意：** 

给出 $n$ 个点，点权为 $a_i$ ，连接 $i$ 和 $j$ 的边权为 $a_i\oplus a_j$ ，求最小生成树的边权和。

数据范围：$1\le n\le 2\times 10^5,0\le a_i<2^{30}$ .

> 题目难度：CF 2300*

> 知识储备：01-Trie / Boruvka

**解析：**

总之不能做出所有边后用 Kruskal 解决，考虑使用 Boruvka 。可以将所有 $a_i$ 插入 01-Trie 来维护异或最小值。把 01-Trie 看作一个二叉树，不难发现恰有 $n-1$ 个节点有左右儿子。$a_i\oplus a_j$ 时只用考虑两点在其 LCA 之后的异或最值，于是对于有左右儿子的点（即为某两点 LCA 的点）， 找出当前能连的最小边并合并其子树即可。

小技巧是把 $a_i$ 排序后从小到大插入，这样每个节点会对应数组中一段连续区间。

时间复杂度：$O(n\log a_i)$ .

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
const int N=2e5+5,inf=(1<<30);
int n,m,a[N],L[N*32],R[N*32],ch[2][N*32],rt,cnt;
//插入trie
void insert(int &p,int id,int dep){
    if(!p)p=++cnt;
    if(!L[p])L[p]=id;
    R[p]=id;
    if(dep==-1)return;
    insert(ch[(a[id]>>dep)&1][p],id,dep-1);
}
//查询异或最小值
ll Q(int p,int x,int dep){
    if(dep==-1)return 0;
    int v=(x>>dep)&1;
    if(ch[v][p])return Q(ch[v][p],x,dep-1);
    else return Q(ch[v^1][p],x,dep-1)+(1<<dep);
}
#define ls ch[0][p]
#define rs ch[1][p]
ll dfs(int p,int dep){
    if(dep==-1)return 0;
    if(ls&&rs){
        ll val=inf;
        rep(i,L[ls],R[ls]){
            val=min(val,Q(rs,a[i],dep-1)+(1<<dep));
        }
        return dfs(ls,dep-1)+dfs(rs,dep-1)+val;
    }
    if(ls)return dfs(ls,dep-1);
    if(rs)return dfs(rs,dep-1);
    return 0;
}
int main(){
    n=read();
    rep(i,1,n)a[i]=read();
    sort(a+1,a+n+1);
    rep(i,1,n)insert(rt,i,30);
    printf("%lld\n",dfs(rt,30));
    return 0;
}
```

### CF1550E Jumping Around

**题目大意：**

数轴上有 $n$ 个石头，坐标为 $a_1<a_2<\cdots<a_n$ 。初始有青蛙在石头 $s$ 上，每次能向左或右跳 $[d,d+k]$ 区间内的任意距离，但是必须要落在石头上。给 $q$ 个询问，问对于给定的 $k$ ，青蛙是否能跳到石头 $i$ ？

数据范围：$1\le n,q\le 2\times 10^5,1\le s,i\le n,1\le d,k,a_i\le 10^6$ .

> 题目难度：CF 2700*

> 知识储备：Boruvka

**解析：**

不难想到建最小生成树后判断 $s$ 到每个点的路径上的最大权值，最后离线处理询问。唯一的问题在如何在有 $O(n^2)$ 条边的图中做 MST 。

具体地，考虑使用 Boruvka ，由于该题的边权有特点，我们可以把所有位置放入 set 中，每次用 lower_bound 查询离 $a_i+d$ 和 $a_i-d$ 最近的点，并加边。Boruvka 会迭代 $O(\log n)$ 次，而每次加边和合并的总复杂度是 $O(n\log n)$ 的，故总体可以做到 $O(n\log^2 n)$ 。加边可以用双指针优化到 $O(n)$ 。

时间复杂度：$O(n\log^2 n+q)$ 或 $O(n\log n+q)$ .

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
const int N=2e5+5,inf=1e9;
struct Edge{
    int u,v,w;
    friend bool operator <(const Edge &a,const Edge &b){
        if(a.w!=b.w)return a.w<b.w;
        if(min(a.u,a.v)!=min(b.u,b.v))
            return min(a.u,a.v)<min(b.u,b.v);
        return max(a.u,a.v)<max(b.u,b.v);
    }
};
int n,q,s,D,fa[N],mn[N];
vector<int>comps[N];
bool merge(int u,int v){
    u=fa[u],v=fa[v];
    if(u==v)return 0;
    if(comps[u].size()<comps[v].size())swap(u,v);
    for(int x:comps[v]){
        fa[x]=u;
        comps[u].pb(x);
    }
    comps[v].clear();
    return 1;
}

vector <pii> G[N];
void dfs(int u,int p,int d){
    mn[u]=d;
    for(auto e:G[u])if(e.fi!=p){
        dfs(e.fi,u,max(d,e.se));
    }
}
int main(){
    n=read(),q=read(),s=read()-1,D=read();
    vector <int> a(n);
    rep(i,0,n-1){
        comps[i]=vector<int>(1,i);
        a[i]=read();
        fa[i]=i;
    }
    vector<int>idx(a[n-1]+1);
    rep(i,0,n-1)idx[a[i]]=i;
    int cnt=n;
    set<int>pos(a.begin(),a.end());
    while(cnt>1){
        vector <Edge> tmp;
        for(const vector<int> &comp:comps)if(!comp.empty()){
            for(int i:comp)pos.erase(a[i]);
            Edge mn={-1,-1,inf};
            for(int i:comp){//找最短边
                for(int d:{-D,D}){
                    auto it = pos.lower_bound(a[i]+d);
                    if(it!=pos.end()){
                        int dis=abs(abs(a[i]-*it)-D);
                        mn=min(mn,{i,idx[*it],dis});
                    }
                    if(it!=pos.begin()){
                        it--;
                        int dis=abs(abs(a[i]-*it)-D);
                        mn=min(mn,{i,idx[*it],dis});
                    }
                }
            }
            for(int i:comp)pos.insert(a[i]);
            tmp.pb(mn);
        }
        for(auto e:tmp){//加边
            if(merge(e.u,e.v)){
                --cnt;
                G[e.u].pb({e.v,e.w});
                G[e.v].pb({e.u,e.w});
            }
        }
    }
    dfs(s,-1,0);
    while(q--){
        int i=read()-1,k=read();
        puts(mn[i]<=k?"Yes":"No");
    }
    return 0;
}
```

