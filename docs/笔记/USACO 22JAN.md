> 赛时爆炸了，很多平常熟悉的套路都没想起来，但题目还是要补的不是吗？

### T1

题意：给一个长度为 $N$ 的数组 $a$ ，若相邻两数之差的绝对值不超过 $K$ 则可以交换，问能得到的所有 $a$ 中字典序最小的一个。 

数据范围：$1\le N,K\le 10^5,1\le a_i\le 10^9$

发现 $|a_i-a_j| > K$ 的数无法交换，于是找到所有这样的限制做拓扑排序。如何做？正常最小拓扑序是把所有入度为0的点放入优先队列中，然后每次取堆顶的点并从图中删除。时间复杂度为 $O(NK)$ 。

思考一下如何优化，首先可以对原数组离散化，用一个线段树来维护当前可能的最小位置，每次取出作为答案并更新与其有连边的点的入度即可。具体地，若当前点的权值为 $u$ ，则 $[1,m]\setminus[u-K+1,u+K-1]$ 中的所有权值对应的点的入度 $-1$ 。需要一个带懒标记线段树做区间更新。

时间复杂度：$O(N\log N)$

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define vi vector<int>
#define fi first
#define se second
#define pb push_back
#define debug(...) fprintf(stderr, __VA_ARGS__)
#define ALL(x) x.begin(),x.end()
#define sz(x) int(x.size())
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2e5+5;
int n,K,a[N],val[N],deg[N];
unordered_map <int,int> vis;
#define ls (p<<1)
#define rs (ls|1)
#define mid ((L+R)>>1)
pii t[N<<2];
int tag[N<<2];
inline pii Min(const pii a,const pii b){
    if(a.se == b.se)return make_pair(min(a.fi,b.fi),a.se);
    return a.se < b.se ? a : b;
}
void build(int p,int L,int R){
    if(L == R){
        t[p] = make_pair(L,deg[L]);
        return;
    }
    build(ls,L,mid),build(rs,mid+1,R);
    t[p] = Min(t[ls],t[rs]);
}
void push(int p,int v){tag[p] += v,t[p].se += v;}
void down(int p){
    if(!tag[p])return;
    push(ls,tag[p]),push(rs,tag[p]);
    tag[p] = 0;
}
void upd(int p,int L,int R,int l,int r,int v){
    if(l > R || r < L)return;
    if(l <= L && R <= r){
        push(p,v);
        return;
    }
    down(p);
    upd(ls,L,mid,l,r,v),upd(rs,mid+1,R,l,r,v);
    t[p] = Min(t[ls],t[rs]);
}
int T[N];
void add(int x){for(;x <= n;x += x & -x)T[x]++;}
int Q(int x){
    int res = 0;
    for(;x;x -= x & -x)res += T[x];
    return res;
} 
int main(){
    n = read(),K = read();
    rep(i,1,n)a[i] = read(),val[i] = a[i];
    sort(val+1,val+n+1);
    rep(i,1,n){
        vis[a[i]]++;
        a[i] = lower_bound(val+1,val+n+1,a[i]) - val + vis[a[i]] - 1;
    }
    //calculating in-degree
    rep(i,1,n){
        int x = lower_bound(val+1,val+n+1,val[a[i]]-K) - val - 1;
        int y = lower_bound(val+1,val+n+1,val[a[i]]+K+1) - val - 1;
        deg[a[i]] = (i-1) + Q(x) - Q(y);
        add(a[i]);
    }
    build(1,1,n);
    rep(i,1,n){
        int u = t[1].fi;
        cout << val[u] << '\n';
        int x = lower_bound(val+1,val+n+1,val[u]-K) - val - 1;
        //cout << 1 << ' ' << x << '\n';
        int y = lower_bound(val+1,val+n+1,val[u]+K+1) - val - 1;
        //cout << y+1 << ' ' << n << '\n';
        //cout << u << ' ' << u << '\n';
        upd(1,1,n,1,x,-1);
        upd(1,1,n,y+1,n,-1);
        upd(1,1,n,u,u,n);
    }
    return 0;
}
```

### T2

题意：给一个长度为 $N$ 的数组 $a$ ，相邻两项若差不大于1则可以交换，问能得到的不同 $a$ 个数，答案模 $10^9+7$ 。

数据范围：$1\le N\le 5000,1\le a_i\le10^9$ 

计数问题基本就是 dp 了。发现从上一题中的不大于 $K$ 变成了不大于 $1$ ，必然有猫腻。由于差大于 $1$ 的数很多，所以大部分数之间是不能够对换位置的，考虑从这一点入手解决问题。

然后此时如果观察到了奇数之间不可能调换位置（除非两数相同，但是这样的调换没有意义），偶数也同理。于是设计 dp 状态为 $dp[i][j]$ 表示当前确定了前 $i+j$ 个位置，有 $i$ 个奇数，$j$ 个偶数。对每个数，预处理它最远能与它不同奇偶性的哪个数交换即可。转移是简单的。

时间复杂度：$O(N^2)$ 。

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define vi vector<int>
#define fi first
#define se second
#define pb push_back
#define debug(...) fprintf(stderr, __VA_ARGS__)
#define ALL(x) x.begin(),x.end()
#define sz(x) int(x.size())
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=5e3+5,P = 1e9+7;
int n,a[N],pos[N];
void add(int &x,const int y){x += y;if(x > P)x -= P;}
void sub(int &x,const int y){x -= y;if(x < 0)x += P;}
void solve(){
    n = read();
    vector <int> even,odd;
    rep(i,1,n){
        a[i] = read();
        if(a[i] & 1){
            pos[i] = sz(odd);
            odd.pb(i);
        }else{
            pos[i] = sz(even);
            even.pb(i);
        }
    }
    vector <int> pre(n+1);
    rep(i,1,n){
        if(a[i] & 1)pre[i] = sz(even);
        else pre[i] = sz(odd);
        rep(j,i+1,n)if(((a[i] - a[j]) & 1) && abs(a[i] - a[j]) > 1){
            pre[i] = min(pre[i],pos[j]);
        }
    }
    vector <vi> dp(sz(even) + 1,vi(sz(odd) + 1));
    dp[0][0] = 1;
    rep(i,0,sz(even))rep(j,0,sz(odd)){
        if(!dp[i][j])continue;
        if(i < sz(even) && j <= pre[even[i]])
            add(dp[i+1][j],dp[i][j]);
        if(j < sz(odd) && i <= pre[odd[j]])
            add(dp[i][j+1],dp[i][j]);
    }
    cout << dp[sz(even)][sz(odd)] << '\n';
}
int main(){
    int T = read();
    while(T--)solve();
    return 0;
}
```

### T3

题意：给定 $n$ 组向量，求每一组中选出一个向量，加起来后最大的长度。

数据范围：向量数不超过 $10^5$ ，$|x|,|y|\le \frac{10^9}{N}$ 。

我们维护当前所有可能答案构成的点集，有

性质1：如果一个点不在该点集的凸包上，那么这个点离原点的距离不是最大的。

对每一组向量我们都可以求出它的凸包。对于第 $i$ 组向量，我们将它的凸包与前 $i-1$ 组得到的答案凸包合并，用 Minkowski 和即可。

时间复杂度：$O(N\log N)$ 。

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define vi vector<int>
#define fi first
#define se second
#define pb push_back
#define debug(...) fprintf(stderr, __VA_ARGS__)
#define ALL(x) x.begin(),x.end()
#define sz(x) int(x.size())
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2e5+5;
struct node{
    ll x,y;
    node operator - (const node a){return (node){x-a.x,y-a.y};}
    node operator + (const node a){return (node){x+a.x,y+a.y};}
    ll operator * (node a)const{return x*a.y - y*a.x;}//cross pro
    ll len()const{return x*x + y*y;}
}O;
bool cmpy(const node &a,const node &b){return a.y == b.y ? a.x < b.x : a.y < b.y;}
bool cmp(const node &a,const node &b){return a*b == 0 ? a.len() < b.len() : a*b > 0;}
int n,m,q,stk[N],top,tot;
void Convex(vector <node> &A){
    sort(A.begin(),A.end(),cmpy);
    O = A[0];
    stk[top=1] = 0;
    rep(i,0,sz(A)-1)A[i] = A[i] - O;
    sort(A.begin()+1,A.end(),cmp);//极角排序
    rep(i,1,sz(A)-1){
        while(top > 1 && (A[i] - A[stk[top-1]]) * (A[stk[top]] - A[stk[top-1]]) >= 0)top--;
        stk[++top] = i;
    }
    rep(i,1,top)A[i-1] = A[stk[i]] + O;
    A.resize(top);
    //rep(i,0,sz(A)-1)cout << A[i].x << ' ' << A[i].y << '\n';
    A.pb(A[0]);
}
vector <node> A,B,vec[N],dA,dB,ans,tmp;
void Minkowski(vector <node> &A,vector <node> &B){
    dA.resize(sz(A)),dB.resize(sz(B));
    rep(i,0,sz(A)-1)dA[i] = A[(i+1)%sz(A)] - A[i];
    rep(i,0,sz(B)-1)dB[i] = B[(i+1)%sz(B)] - B[i];
    ans.pb(A[0] + B[0]);
    int a = 0,b = 0;
    while(a < sz(A) && b < sz(B)){
        if(dA[a] * dB[b] >= 0)ans.pb(ans.back() + dA[a++]);
        else ans.pb(ans.back() + dB[b++]);
    }
    while(a < sz(A))ans.pb(ans.back() + dA[a++]);
    while(b < sz(B))ans.pb(ans.back() + dB[b++]);
}
int main(){
    n = read();
    rep(i,1,n){
        int x = read();
        rep(j,1,x)vec[i].pb((node){read(),read()});
        Convex(vec[i]);
    }
    ans = vec[1];
    rep(i,2,n){
        tmp = ans;ans.clear();
        Minkowski(vec[i],tmp);
        Convex(ans);
    }
    ll len = 0;
    for(auto t : ans)len = max(len,t.len());
    cout << len << '\n';
    return 0;
}
```



赛后总结：

这次比赛对时间的把控极为失败，不应该因为看到了熟悉/感觉能做的题目就一直死磕在上面，这对比赛是极其不利的，希望以后能学会合理利用时间。除了T3的计算几何对USACO是相对新颖的考点，另外两题并没有什么新的东西，T3也容易在了解一点知识后套模板解决。