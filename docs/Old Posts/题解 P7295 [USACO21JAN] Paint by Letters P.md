**题目大意：**

给定一个 $n\times m$ 的网格图，每个格子有一个颜色，若相邻格子颜色相同则连一条边。一共有 $q$ 个询问，对于每个询问 $(x_1,y_1,x_2,y_2)$ ，希望求出该子矩形中连通块的数量。

数据范围：$1\le n,m,q\le 1000,1\le x_1\le x_2\le N,1\le y_1\le y_2\le M$ .

> 知识储备：平面图欧拉定理
>
> 定义 $F$ 为 $G$ 的面数，$E$ 为边数，$V$ 为顶点数，$C$ 为连通分量个数，我们有
> $$
> F=E-V+C+1
> $$

> 题目难度：省选+/USACO Platinum+

**解析：**

一道结论题...没有学过相关知识点只能放弃，了解的话就不算太难。

由上述定理，我们可以正常连边求连通块数，也可以考虑下面这种建图方法，思路大题相同。

若相邻两格颜色不同，则在边界处连一条边。例如题目中给出的 $3\times 3$ 的画布，

```
AAB
BBA
BBB
```

建图后则会变为

```
.-.-.-.
|   | |
.-.-.-.
|   | |
. . .—.
|     |
.-.-.-.
```

此时有
$$
F=18-16+2+1=5
$$
不难发现需要去掉图形外面的那面，于是答案为 $F-1=E-V+C$ . 而 $E$ 可以建图后用二维前缀和求出，$V$ 可以直接计算，于是我们考虑如何处理 $C$ .

标记每个连通分量最左上的点为特殊点，然后询问中的矩形内标记点的个数也可以二维前缀和解决。然而这也算入了连通分量不完全属于该区域，而标记点属于该区域的点。注意到这样的连通分量一定过边界上某个点，遍历边界并从 $C$ 中减去它即可。

时间复杂度：$O(nm+(n+m)q)$ .

```cpp
#include <bits/stdc++.h>
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define per(i,a,b) for(int i=(a);i>=(b);--i)
#define pii pair<int,int>
#define fi first
#define se second
#define ll long long
using namespace std;
inline ll read(){
    ll x=0,f=1;char ch=getchar();
    while (!isdigit(ch)){if (ch=='-') f=-1;ch=getchar();}
    while (isdigit(ch)){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
const int N=2e3+5,d[4][2]={{1,0},{-1,0},{0,1},{0,-1}};
int n,m,q;
char col[N][N];
pii spe[N][N];
int s[N][N],v[N][N],h[N][N];
bool in(int x,int y,int nx,int ny){
    if(nx<0||ny<0||nx>n+1||ny>m+1)return 0;
    if(nx==x+1)return col[x][y]!=col[x][y-1];
    if(nx==x-1)return col[nx][y]!=col[nx][y-1];
    if(ny==y+1)return col[x][y]!=col[x-1][y];
    return col[x][ny]!=col[x-1][ny];
}
void dfs(int x,int y){//建图
    rep(i,0,3){
        int nx=x+d[i][0],ny=y+d[i][1];
        if(in(x,y,nx,ny)){//如果两点间有边
            if(nx==x+1)v[x][y]=1;
            else if(nx==x-1)v[nx][y]=1;
            else if(ny==y+1)h[x][y]=1;
            else h[x][ny]=1;
            if(!spe[nx][ny].fi){
                spe[nx][ny]=spe[x][y];
                dfs(nx,ny);
            }
        }
    }
}
int sum(int f[N][N],int x,int y,int a,int b){
    return f[a][b]+f[x][y]-f[x][b]-f[a][y];
}
bool flg[N][N];
bool out(int i,int j,int x,int y,int a,int b){
    int nx=spe[i][j].fi,ny=spe[i][j].se;
    if(nx>x&&nx<=a&&ny>y&&ny<=b&&!flg[nx][ny]){
        flg[nx][ny]=1;
        return 1;
    }
    return 0;
}
void rem(int x,int y){
    int nx=spe[x][y].fi,ny=spe[x][y].se;
    flg[nx][ny]=0;
}
int main(){
    n=read(),m=read(),q=read();
    rep(i,1,n){
        scanf("%s",col[i]+1);
    }
    rep(i,1,n+1)rep(j,1,m+1){
        if(!spe[i][j].fi){
            s[i][j]=1;
            spe[i][j]={i,j};
            dfs(i,j);
        }
    }
    rep(i,1,n+1)rep(j,1,m+1){
        s[i][j]+=s[i][j-1];
        h[i][j]+=h[i][j-1];
        v[i][j]+=v[i][j-1];
    }
    rep(i,1,n+1)rep(j,1,m+1){
        s[i][j]+=s[i-1][j];
        h[i][j]+=h[i-1][j];
        v[i][j]+=v[i-1][j];
    }
    while(q--){
        int x=read(),y=read(),a=read(),b=read();
        //这里 E 和 V 均去掉了外面一圈，答案不变
        int E=sum(v,x-1,y,a,b)+sum(h,x,y-1,a,b);
        int V=(a-x)*(b-y),C=sum(s,x,y,a,b);
        rep(i,x,a+1){
            if(out(i,b+1,x,y,a,b))C--;
            if(out(i,y,x,y,a,b))C--;
        }
        rep(i,y,b+1){
            if(out(a+1,i,x,y,a,b))C--;
            if(out(x,i,x,y,a,b))C--;
        }
        rep(i,x,a+1){
            rem(i,b+1);rem(i,y);
        }
        rep(i,y,b+1){
            rem(x,i);rem(a+1,i);
        }
        //printf("%d %d %d\n",E,V,C);
        printf("%d\n",E-V+C+1);
    }
    return 0;
}
```



