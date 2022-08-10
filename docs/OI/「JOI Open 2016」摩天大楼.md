题目链接：https://loj.ac/p/2743

>  题目难度：USACO P/省选

好像是暑假之前在 KarL05 推荐给我的dp博客里面看到的题目，但是因为太懒一直没有做，现在凭不完整的记忆把正解重新写出来。

思路：

估一下复杂度必然和 $N,L$ 有关，并且题目要求带限制的排列数，不难想到解法应该是某种dp。由于要求 $A$ 中一些数差值的和（下文记为费用），不妨先将 $A$ 数组排序。接下来，考虑如何去掉绝对值，我们可以从小到大将数插入排列，类似的trick叫 **Component dp** 或者 **插入dp**。

假设现在插入到 $A_i$ ，如何计算插入后对费用的变化呢？可以差分计算，于是记 $dp_{i,j,k,l}$ 为填到第 $i$ 个数，有 $j$ 个连通分量，当前费用为 $k$ ，以及排列的头尾被填入了几个。具体地：

- 插在两个连通块中间并将两块合并，有 $j-1$ 种方案。
- 新增一个连通块，如果头和尾没有放就可以放，共有 $j+1-l$ 种方案。
- 放在一个连通块的头或尾部，并且不改变 $l$ ，有 $2j-s$ 种方案。
- 放在整个排列的头或尾部，如果当前排列已经填入了数，则可以看作加入了这个排列中最左的连通块的左端，或最右连通块的右端。有 $2-s$ 种方案。 

对上述情况分别转移即可，最后注意到 $i$ 这一维可以滚动数组优化。

时间复杂度：$O(N^2L)$ 。

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
#define ALL(x) x.begin(),x.end()
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int MN=105,ML=1005,P=1e9+7;
void upd(int &a,int b){a+=b;if(a>P)a-=P;}
int N,L,A[MN],f[2][3][MN][ML],ans;
#define now (i&1)
#define pre (now^1)
int main(){
    N=read(),L=read();
    rep(i,1,N)A[i]=read();
    if(N==1){puts("1");return 0;}
    sort(A+1,A+N+1);
    A[0]=0;
    f[0][0][0][0]=1;
    rep(i,1,N){
        rep(s,0,2)rep(j,0,i-1)rep(k,0,L)f[now][s][j][k]=0;
        rep(j,0,i-1)rep(s,0,min(2,2*j)){
            int w=(2*j-s)*(A[i]-A[i-1]);
            rep(k,0,L-w){
                int val=f[pre][s][j][k];
                if(!val)continue;
                upd(f[now][s][j+1][k+w],1ll*(j+1-s)*val%P);
                if(j>1)upd(f[now][s][j-1][k+w],1ll*(j-1)*val%P);
                if(j&&s<2)upd(f[now][s+1][j][k+w],1ll*(2-s)*val%P);
                if(s<2)upd(f[now][s+1][j+1][k+w],1ll*(2-s)*val%P);
                if(j)upd(f[now][s][j][k+w],1ll*(2*j-s)*val%P);
            }
        }
    }
    rep(i,0,L)upd(ans,f[N&1][2][1][i]);
    printf("%d\n",ans);
    return 0;
}
```

