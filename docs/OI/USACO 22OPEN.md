事实证明USACO题目难度不再严格升序排列，倒序开题人赢麻了（

说明：最近有点忙，没来得及写题解qwq。不过这场比赛喜提 USA rk3 并且混进队，值得纪念一波。

### T1

题意：给定一个长度为 $N$ 的序列 $a$ ，每次操作可以将两个相邻数 $x,y$ 合并为 $\max(x,y)+1$ ，目标是最小化最后剩下的数。求对数列的所有 $\frac{N(N+1)}{2}$ 个连续子序列，所有答案的和。

数据范围：$1\le N\le 2^{18},1\le a_i\le 10^6$

> 评价：个人认为是本场最大毒瘤，但是做出来的人数莫名和T2相当。重点应该是先从子任务中的倍增 dp 入手，然后观察到极大的子段不多就做出来了。

首先有显然的区间dp：
$$
f_{l,r}=\min_k{\max(f_{l,k},f_{k+1,r})}+1
$$
直接暴力是 $O(N^3)$ ，发现单调性就可以 $O(N^2)$ 。

思考怎么合并是最优的，当 $N$ 为 2 的幂，我们可以每次将 $(1,2),(3,4),\dots,(N-1,N)$ 这样的组合并，不难发现每次数减半，得到的答案最大为 $\max{a_i}+\log N$ 。故一段区间的答案范围在 $[x,x+\log N]$ 之间。

设 $f_{i,k}$ 为最大满足 $[i,f_{i,k}]$ 合并出 $k$ 的右端点。 有 $f[i][k+1]=f[f[i][k]+1][k+1]$ 。统计答案时枚举 $i,k$ 即可，这样做的复杂度为 $O(N\max a_i)$ ，可以打一档部分分。

考虑优化 dp 的过程。称一个子段是**极大的**，若它向左或向右拓展都会使它合并的值增加。

> 引理：极大子段个数为 $f(N)=O(N\log N)$ 。
>
> 证明：考虑原序列的笛卡尔树，设序列的其中一个最大元素在位置 $p$ 。有
> $$
> f(N)\le f(p-1)+f(N-p)+C
> $$
> 其中 $C$ 为原序列的包含位置 $p$ 的最大子段数量。下证
> $$
> C\le O(p\log(\frac{N}{p}))
> $$
> 只需发现值为 $a_p+k$ 的包含位置 $p$ 的子段不超过 $\min(p,2^k)$ ，求和即得。

用 set 记录所有值为 $v$ 的极长子段后，用并查集维护合并区间即可。

时间复杂度：$O(N\log^2 N)$

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define vi vector<int>
#define fi first
#define se second
#define pb push_back
#define ALL(x) x.begin(),x.end()
#define sz(x) int(x.size())
#define ll long long
using namespace std;
const int N = 3e5 + 5,M = 1e6 + 50;
struct DSU{
    vector <int> p,sz;
    void init(int n){
        p.resize(n);sz.assign(n,1);
        iota(p.begin(),p.end(),0);
    }
    int find(int x){return p[x] == x ? x : p[x] = find(p[x]);}
    void merge(int u,int v){
        u = find(u),v = find(v);
        if(u == v)return;
        if(sz[u] > sz[v])swap(u,v);
        p[u] = v,sz[v] += sz[u],sz[u] = 0;
    }
}d;
int n;
int main(){
    cin.tie(0)->sync_with_stdio(0);
    cin >> n;
    vi a(n);for(auto &x : a)cin >> x;
    vector <vi> vec(M);
    rep(i,0,n-1)vec[a[i]].pb(i);
    vector <ll> cnt(M);
    vi R(n,-1),L(n+1,-1);
    auto getR = [&](int x){
        if(x == n)return n;
        return R[d.find(x)];
    };
    set <int> alive;
    ll ans = 0,now = 0;//the amount of values <= i
    d.init(n);
    rep(i,1,M-1){
        vector <int> dead,remove;
        for(int x : alive){
            int r = getR(x);
            int nxtR = max(r,r == n ? -1 : getR(r));
            if(nxtR == r)dead.pb(x);
            else{
                if(L[nxtR] != -1)dead.pb(x);
                else{
                    L[nxtR] = x;
                    remove.pb(nxtR);
                }
                now += 1ll * (nxtR - r) * d.sz[d.find(x)];
                R[d.find(x)] = nxtR;
            }
        }
        for(int x : dead){
            alive.erase(x);
            if(L[getR(x)] == -1)L[getR(x)] = x;
            else d.merge(L[getR(x)],x);
        }
        for(int x : remove)L[x] = -1;
        for(int x : vec[i]){
            ++now;
            R[x] = x + 1;
            alive.insert(x);
            if(L[x] != -1)alive.insert(L[x]);
            L[x] = -1;
        }
        cnt[i] = now;
    }
    per(i,M-1,0)cnt[i] -= cnt[i-1];
    rep(i,1,M-1)ans += cnt[i] * i;
    cout << ans << '\n';
    return 0;
}
```

### T2

题意：有一个 $N$ 个点 $M$ 条边的有向图，两个人在上面做游戏。游戏开始时有两枚棋子分别放在不同的节点上，每一轮游戏中由小B决定移动哪一枚棋子，小H决定沿着哪一条边移动。如果小H在某个时刻无法移动了，则小B获胜；反之小H获胜。

现在有 $Q$ 个询问，每个询问分别给出初始两个棋子的位置，要求回答哪个玩家会获胜。

数据范围：$2\le N\le 10^5,2\le M\le 2\times 10^5,1\le Q\le 10^5$

> 评价：双人博弈中考虑双方的最优策略来简化局面，感觉比较经典。不想偏应该蛮好做的？

出度为0显然死，于是在反图上先拓扑排序把所有必死点找出来，剩下的点出度显然都不小于1。再合并所有出度等于1的点和出边所指向的点。最后剩下的点出度都不小于2，此时若两个棋子在不同位置，B 无法把 H 骗进死路或者用一个棋子堵住另一个，H 赢。两个过程可以类似拓扑排序处理，合并两点的边集用set进行启发式合并，用并查集维护等价关系即可。

时间复杂度 $O(N\log N)$ 。

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define vi vector<int>
#define fi first
#define se second
#define pb push_back
#define ALL(x) x.begin(),x.end()
#define sz(x) int(x.size())
#define ll long long
using namespace std;
const int N = 1e5 + 5;
int n,m,q,fa[N],out[N];
int find(int x){return fa[x] == x ? x : fa[x] = find(fa[x]);}
set <int> G[N],R[N];
bool chk(int x,int y){
    x = find(x),y = find(y);
    if(!fa[x] || !fa[y])return 1;//dead end
    if(x == y)return 1;
    return 0;
}
int main(){
    cin.tie(0)->sync_with_stdio(0);
    cin >> n >> m;
    int u,v;
    rep(i,1,m){
        cin >> u >> v;
        out[u]++;
        R[v].insert(u);//reverse graph
        G[u].insert(v);
    }
    queue <int> Q;
    rep(i,1,n){
        fa[i] = i;
        if(sz(G[i]) <= 1)Q.push(i);
    }
    while(!Q.empty()){//topo
        int u = Q.front();
        Q.pop();
        int v = 0;
        for(int x : G[u])if(find(x) != 0){
            v = find(x);
            break;
        }
        if(v == 0){//dead end
            fa[u] = 0;
            for(int x : R[u]){
                --out[x];
                if(out[x] == 1)Q.push(x);
            }
        }else{
            //merge points of outdeg = 1
            if(u == v)continue;
            R[v].erase(u);
            if(sz(R[u]) > sz(R[v]))swap(R[u],R[v]);
            fa[u] = v;
            for(int x : R[u]){
                if(R[v].find(x) != R[v].end()){
                    --out[x];
                    if(out[x] == 1)Q.push(x); 
                }else R[v].insert(x);
            }
        }
        R[u].clear();
    }
    cin >> q;
    while(q--){
        cin >> u >> v;
        if(chk(u,v))putchar('B');
        else putchar('H');
    }
    return 0;
}
```

### T3

题意：给定长度为 $N$ 的一个排列 $p$ 和一个长度为 $N-1$ 的字符串 $S$ ，$S$ 只包含字母 U 和 D 。要求找出 $p$ 最长的子序列 $a_0,a_1,\dots,a_K$ ，满足 $S_i = U$ 时 $a_{i-1} < a_i$ ，反之 $a_{i-1} > a_i$ 。

数据范围：$2\le N\le 3\times 10^5$

> 评价：这是铂金题？

设 $f_{i,j}$ 为考虑到第 $i$ 位，与 $S$ 的前 $j$ 位匹配后最后一位最优是多少，可以做到 $O(N^2)$ 。

考虑优化，问题在原来的状态设计需要对于每个状态分别更新。不妨设 $f_i,g_i$ 为以权值 $i$ 结尾，下一位钦定为 U/D 时能匹配的最长前缀。这样一来每次转移可以使用线段树查询最优的方案了。时间复杂度为 $O(N\log N)$ 。

代码：

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define vi vector<int>
#define fi first
#define se second
#define pb push_back
#define ll long long
using namespace std;
const int N = 3e5 + 5;
#define mid ((L + R) >> 1)
#define ls (p << 1)
#define rs (ls | 1)
struct SegTree{
    int mx[N << 2];
    void pushup(int p){mx[p] = max(mx[ls],mx[rs]);}
    void modify(int p,int L,int R,int x,int v){
        if(L == R){
            mx[p] = max(mx[p],v);
            return;
        }
        x <= mid ? modify(ls,L,mid,x,v) : modify(rs,mid + 1,R,x,v);
        pushup(p);
    }
    int Q(int p,int L,int R,int l,int r){
        if(r < L || R < l)return 0;
        if(l <= L && R <= r)return mx[p];
        return max(Q(ls,L,mid,l,r),Q(rs,mid + 1,R,l,r));
    }
}f,g;
int n,a[N];
string s;
int main(){
    cin.tie(0)->sync_with_stdio(0);
    cin >> n;
    rep(i,1,n)cin >> a[i];
    cin >> s;
    rep(i,1,n){
        int U = f.Q(1,1,n,1,a[i]),D = g.Q(1,1,n,a[i],n);
        U++,D++;
        if(s[U - 1] == 'U')f.modify(1,1,n,a[i],U);
        else g.modify(1,1,n,a[i],U);
        if(s[D - 1] == 'U')f.modify(1,1,n,a[i],D);
        else g.modify(1,1,n,a[i],D);
    }
    int ans = max(f.Q(1,1,n,1,n),g.Q(1,1,n,1,n));
    cout << ans - 1 << '\n';
    return 0;
}
```









