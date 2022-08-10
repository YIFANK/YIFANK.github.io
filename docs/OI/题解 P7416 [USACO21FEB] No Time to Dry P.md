考虑一段区间 $[l,r]$ 最多要涂 $r-l+1$ 笔，然而有些笔是“多余的”。不难发现，对于每对 $(x,y)$ 满足 $A_x=A_y<\min_{x<i<y}A_i$ （一对相同颜色且中间的颜色更深），我们可以少涂一笔。那么记区间内的 $(x,y)$ 对个数为$cnt$，所求的答案就是 $r-l+1-cnt$。

我们可以通过维护一个单调栈来找出所有这样的点对，栈内元素单调递增。如果新插入的元素和栈顶元素相等，那么它就是一对符合要求的点，更新 $cnt$ 和栈顶。最后对这些询问按照右端点坐标排序， 用树状数组来维护一段前缀的 $cnt$ ，离线处理每组询问即可。

时间复杂度： $O((N+Q)\log N)$ 。

代码：

```cpp
#include <bits/stdc++.h>
#define f first
#define s second
#define pi pair<int,int>
using namespace std;
const int N=2e5+5;
int n,q,a[N],stk[N],top,ans[N],bit[N];
inline void I(int x){for(;x<=n;x+=(x&-x))bit[x]++;}
inline int Q(int x){int res=0;for(;x;x-=(x&-x))res+=bit[x];return res;}
vector <pi> query[N];
int main(){
    scanf("%d%d",&n,&q);
    for(int i=1;i<=n;i++)scanf("%d",&a[i]);
    for(int i=1,l,r;i<=q;i++){
        scanf("%d%d",&l,&r);
        query[r].push_back({l,i});
    }
    for(int i=1;i<=n;i++){
        while(top&&a[stk[top]]>a[i])top--;
        if(top&&a[stk[top]]==a[i]){
            I(stk[top]);
            stk[top]=i;
        }else stk[++top]=i;
        if(query[i].size()){
            for(int j=0;j<query[i].size();j++){
                pi t=query[i][j];
                ans[t.s]=i-t.f+1-(Q(i)-Q(t.f-1));
            }
        }
    }
    for(int i=1;i<=q;i++)printf("%d\n",ans[i]);
    return 0;
}
```