**题目大意：**

给一块 $N\times N$ 的网格，其中有 $N$ 个特殊格子，所在的行和列互不相同。一个位于 $(i,j)$ 的特殊格有以下效果：

- 对所有 $0\le x\le i,0\le y\le j$ 的 $(x,y)$ 带来效果 $1$ 。
- 对所有 $i\le x\le N,j\le y\le N$ 的 $(x,y)$ 带来效果 $2$ 。

问网格中有多少个子矩形，满足所有格都被效果 $1$ 和 $2$ 覆盖。

数据范围：$1\le N\le 10^5$ .

> 题目难度：省选/USACO Platinum

> 知识储备：基础算法

**解析：**

首先考虑每一行中哪些格子会同时受到效果 $1$ 和 $2$ 。由于每次影响的是一个连续的区间，显然行上同时受到影响的区间也是连续的。设 $l_i,r_i$ 为这段区间，那么不难发现 $l_i$ 为第 $i$ 行以上最靠左的特殊格横坐标，$r_i$ 为第 $i$ 行以下最靠右的特殊格横坐标。可以简单预处理出所有行的 $l_i,r_i$ 。

枚举矩形的右下角 $(i,j)$ 和左上角 $(x,y)$ ，有
$$
\sum_{i=1}^{n}\sum_{j=l_i}^{r_i}\sum_{y=l_i}^{j-1}\sum_{x=up_y}^{i-1}1
$$
简单推一下式子优化：
$$
\begin{aligned}
&=\sum_{i=1}^{n}\sum_{j=l_i}^{r_i}\sum_{y=l_i}^{j-1}(i-up_y)\\
&=\sum_{i=1}^{n}(i\times\frac{(r_i-l_i)(r_i-l_i+1)}{2}-\sum_{y=l_i}^{j-1}\sum_{j=y+1}^{r_i}up_y)\\
&=\sum_{i=1}^{n}(i\times\frac{(r_i-l_i)(r_i-l_i+1)}{2}-\sum_{y=l_i}^{j-1}up_y\times(r_i-y))\\
\end{aligned}
$$
前缀和优化即可 $O(N)$ 计算。

时间复杂度：$O(N)$ .

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
const int N=2e5+5,P=1e9+7;
int n,a[N],sum1[N],sum2[N];
int ans,l[N],r[N],up[N];
int main(){
    n=read();
    rep(i,1,n){
        int x=read()+1,y=read()+1;
        a[x]=y;
    }
    l[0]=n,r[n+1]=0;
    rep(i,1,n)l[i]=min(l[i-1],a[i]);
    per(i,n,1)r[i]=max(r[i+1],a[i]);
    int pos=r[1];
    rep(i,1,n){
        while(pos&&pos>=l[i]){
            up[pos]=i,pos--;
        }
    }
    rep(i,1,n)sum1[i]=(sum1[i-1]+up[i])%P;
    rep(i,1,n)sum2[i]=(sum2[i-1]+1ll*i*up[i]%P)%P;
    rep(i,1,n){
        ans=(ans+1ll*i*(1ll*(r[i]-l[i])*(r[i]-l[i]+1)/2ll%P)%P
        -1ll*(sum1[r[i]-1]-sum1[l[i]-1]+P)%P*r[i]%P+
        (sum2[r[i]-1]-sum2[l[i]-1]+P)%P)%P;
    }
    printf("%d\n",(ans+P)%P);
    return 0;
}
```

