### T1

题意：给定离散化坐标后的 $n$ 个矩形的位置 $(x_1,y_1,x_2,y_2)$ ，它们将平面分为若干个区域。以下列规则黑白染色：

- 所有矩形的并集的外界是白色
- 相邻区域的颜色不同

一个询问，$T=1$ 时询问总区域数量，$T=2$ 时询问黑白色区域分别的数量。

数据范围：$1\le n\le 10^5$

对于 $T=1$ ，我们运用平面图欧拉定理（或者找规律）：$V-E+F=C+1$ ，发现区域数量等于交点数量加连通块数量加1，扫描线计算交点数量即可，用 set 维护连通块即可。

$T=2$ ，对于 $n\le 2500$ ，可以发现被奇数个矩形覆盖的区域是 $1$ ，偶数个是 $0$ ，将二维区间操作差分为单点修改，最后前缀和算出每个点的颜色再bfs算区域数量。时间复杂度为 $O(n^2)$ 。

实际上并不需要欧拉定理，用树状数组维护颜色段数，set维护每个边连通块最左一条边的上下端点，就可以处理多个连通块的情况。idea 来自 @SharpnessV 。

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
const int N=2e5+5;
int n,T,op[N],L[N],R[N],t[N];
ll c[2];
void ins(int x,int v){
    for(;x <= n + n;x += x & -x)t[x] += v;
}
int sum(int x){
    int res = 0;
    for(;x;x -= x & -x)res += t[x];
    return res;
}
void work(int x,int y){
    int cur = sum(x) & 1,cnt = 1 + sum(y) - sum(x);//start color & number of segs
    c[cur] += (cnt + 1) / 2,c[cur ^ 1] += cnt / 2;
}
bool vis[N];
struct node{
    int pos,val,id;
    bool operator <(const node &a)const{return pos < a.pos;}
};
set <node> S;
void check(int x,int y){
    auto cur = S.lower_bound({x,0,0});
    vector <node> v;
    while(cur != S.end()){
        if((*cur).pos > y)break;
        v.pb(*cur);cur++;
    }
    if(v.empty())return;
    sort(v.begin(),v.end(),[](node x,node y){return x.id < y.id;});
    int id = v[0].id;
    for(auto t : v)if(t.id != id){//合并
        if(!vis[t.id]){
            vis[t.id] = 1,c[t.val]--;
            S.erase({L[t.id],0,0}),S.erase({R[t.id] + 1,0,0});
        }
    }
}
int main(){
    cin.tie(0)->sync_with_stdio(0);
    cin >> n >> T;
    rep(i,1,n){
        int x,y,X,Y;
        cin >> x >> y >> X >> Y;
        op[x] = 1,L[x] = y,R[x] = Y - 1;
        op[X] = -1,L[X] = y,R[X] = Y - 1;
    }
    rep(i,1,2*n){
        ins(L[i],op[i]),ins(R[i] + 1,op[i]);
        work(L[i],R[i]);
        check(L[i],R[i] + 1);
        if(op[i] == 1){
            if(sum(L[i]) == sum(R[i])){//新连通块
                int w = (sum(L[i]) & 1) ^ 1;
                c[w]++;
                S.insert({L[i],w,i}),S.insert({R[i] + 1,w,i});
            }
        }else{
            //减去多算的颜色
            c[sum(L[i] - 1) & 1]--,c[sum(R[i] + 1) & 1]--;
            auto cur = S.lower_bound({L[i],0,0});
            if(cur != S.end() && (*cur).pos == L[i]){
                S.erase(cur);S.erase({R[i] + 1,0,0});
            }
        }
    }
    c[0]++;
    if(T == 1)cout << c[0] + c[1] << '\n';
    else cout << c[0] << ' ' << c[1] << '\n';
    return 0;
}
```



### T2

题意：给定一个长度为 $n$ 的序列 $a$ ，你可以进行两种操作：

- 合并：将相邻两个数 $a,b$ 删除并在同一位置插入 $a+b$ 。
- 分裂：将某个数 $a$ 删除并在同一位置插入 $x,y$ ，满足 $x+y = a$ 。

有 $q$ 个询问，对于每个询问中的 $k$ ，求最少操作次数使得序列中所有数都为 $k$ 。

数据范围：$1\le n,q\le 10^5,1\le a_i,k,\sum a_i\le 10^{18}$

首先 $k$ 不为 $S=\sum a_i$ 的因子时显然无解。

如果 $n=1$ ，那么直接砍 $a_1/k - 1$ 刀就行。然后发现原题等价于砍了 $n-1$ 刀的 $S$ 还要几次操作，不难发现还是按照长度为 $k$ 分段砍，但是：

- 若原来的落刀没问题，则少砍一刀
- 反之，需要多一次合并操作

记 $b_i = \sum_{k=1}^i a_i$ ，那么答案显然为：
$$
\frac{S}{k}-1+n-1-2\sum_{i=1}^{n-1}[b_i \equiv 0\pmod k]
$$
 于是我们将原命题转换为给一个长度为 $n-1$ 的序列 $b$ ，每次对于一个 $S$ 的因子 $k$ ，询问 $b$ 中 $k$ 的倍数的个数。把 $b$ 换成 $\gcd(b,S)$ 不影响答案。$10^{18}$ 以内的数 $S$ 的因子个数的最大值是 $O(10^5)$ ，找出 $S$ 的所有素因子，做高维后缀和解决。

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
const int N= 1e6 + 5;
int n,q,pri[N],tot,flg;
bool vis[N];
ll a[N],s[N];
map <ll,int,greater<ll> > mp;
vector <ll> P;
ll gcd(ll x,ll y){return !y ? x : gcd(y,x % y);}
int main(){
    n = read();
    rep(i,2,1000000){
        if(!vis[i])pri[++tot] = i;
        for(int j = 1;j <= tot && i * pri[j] <= 1000000;j++){
            vis[i * pri[j]] = 1;
            if(i % pri[j] == 0)break;
        }
    }
    rep(i,1,n)a[i] = read();
    rep(i,1,n)s[i] = s[i-1] + a[i];
    ll S = s[n];
    rep(i,1,tot){
        if(S % pri[i] == 0){
            while(S % pri[i] == 0)S /= pri[i];
            P.pb(pri[i]);
        }
    }
    ll x = sqrt(S);
    if(x > 1 && x * x == S)P.pb(S/x),flg = 1;
    rep(i,1,n-1){
        s[i] = gcd(s[i],s[n]);
        mp[s[i]]++;
        if(!flg){
            ll x = s[i];
            for(ll p : P)while(x % p == 0)x /= p;
            if(x > 1e6 && flg != 1){
                if(x < S){
                    P.pb(x),P.pb(S/x),flg = 1;
                }else flg = 2;
            }
        }
    }
    if(flg == 2)P.pb(S);
    for(ll p : P){
        for(auto it = mp.begin();it != mp.end();it++){
            if(it->fi % p == 0)mp[it->fi / p] += it->se;
        }
    }
    q = read();
    while(q--){
        ll k = read();
        if(s[n] % k != 0){
            puts("-1");
            continue;
        }
        ll ans = s[n] / k + n - 2 - 2 * mp[k];
        printf("%lld\n",ans);
    }
    return 0;
}
```



### T3

题意：$T$ 组数据，给定一个长度为 $n$ 的只包含1-9的字符串。你可以将串划分成不交的一些区间，区间内的数字在九宫格内需要是单独、相邻、或2*2方格。这些区间内的数字可以任意排列，问能产生的不同串个数。

数据范围：$\sum n\le 10^5$

首先看部分分，若没有 2*2 方格，可以直接设 $f[i][0/1]$ 为当前考虑到第 $i$ 位，有没有和前一位成区间。回到原题，我们需要对不同分划带来同样的串的方案去重，于是状态应该加入前面四个数的排列。时间复杂度应该是大常数 $O(n)$ ？赛时想到这之后就不大会做了（

考虑对于一个串 $B$ ，如何用 dp 判断串 $A$ 是否能操作后变成 $B$ 。设 $f_i=0/1$ 表示前 $i$ 位是否满足条件。若当前考虑到第 $i$ 位，$f_i=1$ 仅当：

- 若 $f_{i-1}=1$ 且 $A_i=B_i$ 。
- 若 $f_{i-2}=1$ 且 $A_{i-1},A_{i}$ 是 $B_{i-1},B_i$ 的排列。
- 若 $f_{i-4}=1$ 且 $A_{i-2},A_{i-3},A_{i-1},A_{i}$ 是 $B_{i-2},B_{i-3},B_{i-1},B_{i}$ 的排列。

将这个 dp 改成自动机，则对于第 $i$ 位只需要记住前三位的数字以及 $f_i$ ，把这些数字一起当成自动机的节点即可。不难发现对于每个 $i$ ，合法节点的数量不多，直接 dp 即可。并且有了自动机之后，直接把 $f_i$ 改成统计个数即可，不同节点代表的信息不同，所以不会算重。

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
const int N=1e5+5,P = 1e9+7;
int num[2048];
const int A[4] = {
    1<<1|1<<2|1<<4|1<<5,1<<2|1<<3|1<<5|1<<6,
    1<<4|1<<5|1<<7|1<<8,1<<5|1<<6|1<<8|1<<9
};
int sta4(int a,int b,int c,int d){return 1<<a|1<<b|1<<c|1<<d;}
int sta2(int a,int b){return 1<<a|1<<b;}
bool insq(int S,int c,int sq){
    if((S & sq) != S)return 0;
    if(num[S] != c)return 0;
    if(find(A,A+4,sq) == A+4)return 0;
    return 1;
}
bool line(int a,int b){
    if(a != b)return 0;
    if(a % 9 == 0){//是否列相邻
        a /= 9;
        return (a & (a-1)) == 0;
    }
    if(a % 3 == 0){//行相邻
        a /= 3;
        if(a & a-1)return 0;
        if(a == 1<<3 || a == 1<<6)return 0;
        return 1;
    }
    return 0;
}
struct S{
    int a3,a2,a1,a0;
    bool operator < (const S &x)const{
        return a3 < x.a3 || a3 == x.a3 && (a2 < x.a2 || a2 == x.a2 && (a1 < x.a1 || a1 == x.a1 && a0 < x.a0));
    }
};
char s[N];
int n,a[N];
inline int c(char ch){return ch - '0';}
map <S,int> f,g;
void solve(){
    scanf("%s",s+1);
    int n = 0;
    while(s[n+1])n++;
    rep(i,1,n)a[i] = s[i] - '0';
    f.clear();
    f[(S){-1,-1,-1,0}] = 1;
    rep(i,1,n){
        g.clear();
        for(auto &pre : f){
            S o = pre.fi;
            rep(x,1,9){
                S nxt;
                nxt.a3 = o.a2 | 1 << x;
                nxt.a2 = o.a1 | 1 << x;
                nxt.a1 = o.a0 | 1 << x;
                nxt.a0 = -1;
                if(o.a3 != -1 && insq(o.a3 | 1 << x,4,sta4(a[i-3],a[i-2],a[i-1],a[i])))nxt.a0 = 0;
                if(nxt.a2 != -1 && line(nxt.a2,sta2(a[i-1],a[i])))nxt.a0 = 0;
                if(nxt.a1 == (1<<a[i]))nxt.a0 = 0;
                if(nxt.a3 != -1 && (i == n || !insq(nxt.a3,3,sta4(a[i-2],a[i-1],a[i],a[i+1]))))nxt.a3 = -1;
                if(nxt.a2 != -1 && (i >= n-1 || !insq(nxt.a2,2,sta4(a[i-1],a[i],a[i+1],a[i+2]))))nxt.a2 = -1;
                if(nxt.a0 != -1 || nxt.a1 != -1 || nxt.a2 != -1 || nxt.a3 != -1){
                    g[nxt] = (g[nxt] + pre.se) % P;
                }
            }
        }
        f = g;
    }
    ll ans = 0;
    for(auto &i : f)if(i.fi.a0 == 0)ans = (ans + i.se) % P;
    cout << ans << '\n';
}
int main(){
    rep(i,1,2047)num[i] = num[i>>1] + (i&1);
    int T = read();
    while(T--)solve();
    return 0;
}
```

