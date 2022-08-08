# Google Kick Start Round H

### A

可以优化到 $O(n+m)$ ，这里随便打了个暴力。

```cpp
const int N=2e5+5;
char s[N],f[N];
int main(){
    int T=read();
    rep(t,1,T){
        scanf("%s%s",s+1,f+1);
        int n=strlen(s+1),m=strlen(f+1),ans=0;
        rep(i,1,n){
            int mx=26;
            rep(j,1,m){
                int k=abs(f[j]-s[i]);
                mx=min(mx,min(k,26-k));
            }
            ans+=mx;
        }
        printf("Case #%d: %d\n",t,ans);
    }
    return 0;
}
```
### B

暴力题，不难发现每种颜色是独立的，分别算段数求和就行，$O(n)$ 。

```cpp
const int N=2e5+5;
char s[N];
bool R(char c){return c=='R'||c=='O'||c=='P'||c=='A';}
bool Y(char c){return c=='Y'||c=='O'||c=='G'||c=='A';}
bool B(char c){return c=='B'||c=='G'||c=='P'||c=='A';}
int main(){
    int T=read();
    rep(t,1,T){
        int ans=0,n=read();
        scanf("%s",s+1);
        rep(i,1,n){
            if(R(s[i])&&!R(s[i-1]))ans++;
        }
        rep(i,1,n){
            if(Y(s[i])&&!Y(s[i-1]))ans++;
        }
        rep(i,1,n){
            if(B(s[i])&&!B(s[i-1]))ans++;
        }
        printf("Case #%d: %d\n",t,ans);
    }
    return 0;
}
```
### C

思维题，暴力做法 $O(n^2)$ ，考虑优化。我们考虑加快扫描区间的速度，具体地，可以用 vector 存下每个可能操作的位置，注意到新生成的数最多和左右各产生一个新的位置，总共最多操作 $n-1$ 次，于是位置数量是 $O(n)$ 的。用链表优化删除，模拟即可。

```cpp
const int N=5e5+5;
char s[N],res[N];
int l[N],r[N];
bool tag[N];
char nc(char c){if(c>='0'&&c<'9')return c+1;return '0';}
vi v[10];
int main(){
    int T=read();
    rep(t,1,T){
        int n=read();
        scanf("%s",s+1);
        rep(i,1,n)l[i]=i-1,r[i]=i+1,tag[i]=0;
        int cnt=0;
        rep(k,'0','9')rep(i,1,n){
            if(s[i]==k&&s[i+1]==nc(k))v[k-'0'].pb(i),++cnt;
        }
        int k=0;
        while(cnt){
            while(v[k].size()){
                int x=v[k].back(),y=r[x];
                v[k].pop_back();
                --cnt;
                if(s[x]!=k+'0'||s[y]!=nc(k+'0'))continue;
                if(l[x])r[l[x]]=y;
                l[y]=l[x],tag[x]=1;
                s[y]=nc(s[y]);
                if(l[y]&&nc(s[l[y]])==s[y]){
                    v[(k+1)%10].pb(l[y]);
                    cnt++;
                }
                if(r[y]<=n&&nc(s[y])==s[r[y]]){
                    v[(k+2)%10].pb(y);cnt++;
                }
            }
            k=(k+1)%10;
        }
        printf("Case #%d: ",t);
        rep(i,1,n)if(!tag[i])cout<<s[i];
        cout<<endl;
    }
    return 0;
}
```
### D

一眼动态 dp 题。转移是显然的。但是由于题目不带修改，我们可以使用树上倍增维护，记 $A_{u,k},B_{u,k}$ 为 $u$ 往上走 $2^k$ 步的祖先是否被染色时，$u$ 被染色的概率。魔改一下 LCA 函数就好了。时间复杂度 $O((n+q)\log n)$ 。

```cpp
const int N=2e5+5,P=1e9+7,M=1e6;
ll n,q,K[N],A[N][18],B[N][18],fa[N][18],dep[N],inv[M+5];
void jump(int &u,int k,pii &a){
    a={(a.fi*A[u][k]+a.se*(P+1-A[u][k]))%P,
    (a.fi*B[u][k]+a.se*(P+1-B[u][k]))%P};
    u=fa[u][k];
}
int calc(int x,int y){
    pii ax={1,0},ay={1,0};
    if(dep[x]<dep[y])swap(x,y);
    per(k,17,0){
        if(dep[x]-(1<<k)>=dep[y])jump(x,k,ax);
    }
    if(x^y){
        per(i,17,0){
            if(fa[x][i]!=fa[y][i]){
                jump(x,i,ax);
                jump(y,i,ay);
            }
        }
        jump(x,0,ax),jump(y,0,ay);
    }
    return (ax.fi*ay.fi%P*K[x]+ax.se*ay.se%P*(P+1-K[x]))%P;
}
void solve(){
    n=read(),q=read(),K[1]=read()*inv[M]%P;
    rep(i,2,n){
        fa[i][0]=read(),A[i][0]=read()*inv[M]%P,B[i][0]=read()*inv[M]%P;
        dep[i]=dep[fa[i][0]]+1;
        K[i]=(K[fa[i][0]]*A[i][0]+(P+1-K[fa[i][0]])*B[i][0])%P;
        rep(j,1,18){
            fa[i][j]=fa[fa[i][j-1]][j-1];
            if(fa[i][j]){
                A[i][j]=(A[i][j-1]*A[fa[i][j-1]][j-1]+B[i][j-1]*(P+1-A[fa[i][j-1]][j-1]))%P;
                B[i][j]=(A[i][j-1]*B[fa[i][j-1]][j-1]+B[i][j-1]*(P+1-B[fa[i][j-1]][j-1]))%P;
            }
        }
    }
    while(q--){
        int u=read(),v=read();
        printf("%d ",calc(u,v));
    }
    puts("");
}
int main(){
    inv[1]=1;
    rep(i,2,M){
        inv[i]=(P-P/i)*inv[P%i]%P;
    }
    int T=read();
    rep(t,1,T){
        printf("Case #%d: ",t);
        solve();
    }
    return 0;
}
```