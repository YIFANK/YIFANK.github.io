# P5978 [CEOI2018]Global warming 题解

题意：给定长度为$n$的数组$a$，你可以将任意一段区间$[l,r]$中的每一个元素$+d$，其中$-x\le d\le x$，问一次操作后的最长上升子序列长度。

数据范围：$1\le n\le 2\times 10^5$。

思路：如果把$[l,r]$区间的数变小，可能会增加到这一段区间和后面$[r+1,n]$接上的LCS长度，但同时可能减少$[1,r]$这段的LCS长度。不难发现，对于同样的$d<0$，操作$[1,r]$这个区间一定比操作$[l,r]$区间更优。

同时，由于操作过后元素的相对大小不变，$[1,r]$减小$d$和$[r+1,n]$增加$d$这两个操作本质上是一样的。因为前缀中元素的相对大小不变，为了LCS更长，显然$d=x$时最优。于是题目就转化成了：考虑对前缀$[1,r]$ 中的元素$-x$，问操作后最长的LCS长度。暴力+LCS可以做到$O(n^2\log n)$，但这不是我们想要的。

操作了一段前缀并不会影响这段前缀内部的LCS，于是我们可以通过dp求解$[1,i]$这段前缀以$a_i$为终点的LCS长度，记为$L_i$。设$R_i$为在$[1,i]$区间修改过后，以$a_i$为起点的LCS，那么答案就是：

$$\max_{i=1}^n L_i+R_i-1$$

时间复杂度：$O(n\log n)$。

代码：

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);i++)
#define per(i,a,b) for(int i=(a);i>=(b);i--)
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2e5+5;
int n,x,a[N],lis[N],L[N],R[N],ans;
int main(){
    n=read(),x=read();
    rep(i,1,n)a[i]=read();
    memset(lis,0x3f,sizeof(lis));
    rep(i,1,n){//计算[1,i]的LIS
        int j=lower_bound(lis,lis+n,a[i])-lis;
        lis[j]=a[i];
        L[i]=j+1;
        ans=max(ans,L[i]);
    }
    memset(lis,0x3f,sizeof(lis));
    per(i,n,1){//计算后缀的下降子序列=以i为起点的上升子序列
        int j=lower_bound(lis,lis+n,-a[i]+x)-lis;
        int jj=lower_bound(lis,lis+n,-a[i])-lis;
        lis[jj]=-a[i];
        ans=max(ans,L[i]+j);
    }
    printf("%d\n",ans);
    return 0;
}
```

