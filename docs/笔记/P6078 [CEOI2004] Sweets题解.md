## P6078 [CEOI2004] Sweets题解

题意：$n$种糖果，第$i$种有$m_i$颗，问有多少种方案从中选出总数$[a,b]$之间颗糖，答案对$2004$取模。

数据范围：$1\le n\le 10,0\le a\le b\le 10^7,0\le m_i\le 10^6$

思路：

生成函数入门题，不难得出总方案的生成函数为：

$$G(x)=\prod_{i=1}^n \frac{1-x^{m_i+1}}{1-x}=(1-x)^{-n}\cdot\prod_{i=1}^{n}(1-x^{m_i+1})$$

注意到$n$很小，因此对于右边的式子我们可以直接暴力求解（这一部分是$O(2^n)$），而左边可以用牛顿二项式定理求解：

$$(1-x)^{-n}=\sum_{i\ge 0}\binom{n-1+i}{i}x^i$$

于是我们得出了这个多项式。现在要求的是$\sum_{i=a}^b[x^i]G(x)$。

枚举$\prod_{i=1}^{n}(1-x^{m_i+1})$中第$k$项系数，设为$c_k$。那么它对答案的贡献是：

$$c_k\sum_{i=a-k}^{b-k}\binom{n-1+i}{i}=c_k(\binom{n+b-k}{b-k}-\binom{n+a-k-1}{a-k-1})$$

最后组合数$\binom{n+b-k}{n}=\dfrac{(n+b-k)!}{n!(b-k)!}$对$2004$取模的细节问题，我们可以直接计算$\dfrac{(n+b-k)!}{(b-k)!}$对$2004\cdot n!$取模再将答案除以$n!$即可。

时间复杂度：$O(2^n+b)$。

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int mod=2004;
int n,l,r;ll a[15],fac=1,sum=0;
inline ll C(int m,int n){
    ll M=mod*fac,ans=1;
    for(int i=n-m+1;i<=n;i++)ans=(ans*i)%M;
    return (ans/fac)%mod;
}
void dfs(int pos,int val,int key,int lim){
    if(key>lim)return;
    if(pos>n){
        sum+=1ll*val*C(n,n+lim-key)%mod;
        sum=(sum+mod)%mod;
        return;
    }
    dfs(pos+1,val,key,lim);dfs(pos+1,-val,key+a[pos]+1,lim);
}
ll solve(int lim){
    sum=0;dfs(1,1,0,lim);
    return (sum%mod+mod)%mod;
}
int main(){
    scanf("%d%d%d",&n,&l,&r);
    for(int i=1;i<=n;i++){
        scanf("%lld",&a[i]);
        fac*=i;
    }
    printf("%lld\n",((solve(r)-solve(l-1))%mod+mod)%mod);
    return 0;
}
```





