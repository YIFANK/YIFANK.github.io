In this blog, I will be sharing some notes related to Berlekamp-Massey Algorithm, a powerful algorithm that will help you to find the shortest linear recurrence relation in a sequence.

### What is linear recurrence?

Though it might be the first time to see such a fancy term, most people have actually encountered linear recurrence relations many times. For example, the sequence


$$
1,2,4,8,16,\dots
$$


is a linear recurrence sequence because of the recurrence relationship


$$
a_n=2\times a_{n-1}
$$



Another famous example would be the Fibonacci numbers $F_0,F_1,F_2,\dots$ where $F_0=0,F_1=1,F_n=F_{n-1}+F_{n-2}$ for $n\ge 2$ . so, the sequence begins


$$
0,1,1,2,3,5,8,\dots
$$


Basically, a sequence $a_0,a_1,\dots,a_{n-1}$ satisfies a linear recurrence relation $c_1,c_2,\dots,c_m$ if for all $i\ge m$, we have


$$
a_i=\sum_{j=1}^m c_ja_{i-j}
$$



 ### Berlekamp-Massey Algorithm

Back to the main topic, how does the algorithm find a linear recurrence relation for a given sequence?

We can use the idea of induction. 

- First, finding such relation for a sequence of length 1 is trivial.
- It is also meaningless to find a recurrence relation for an all-zero sequence, so we want to start with the first non-zero element in the sequence.
- Suppose $a_i$ is the first non-zero element, by definition, the recurrence relation has to begin with $m=i$ . WLOG, set all $c_i$ to be zero for now.
- Suppose for all $0\le i < k$ ,  we now find the linear recurrence relation $C_i$ for the sequence $\{a_0,a_1,\dots,a_i\}$ . We want to find a linear recurrence relation for $\{a_0,a_1,\dots,a_k\}$ .
- Let's say for $C_{k-1}$ , we get $a_k'$ based on the recurrence. If $a_k'=a_k$ , then $C_k=C_{k-1}$ .
- Otherwise, we need to make some adjustments to find the new relation. Say $\Delta = a_k-a_k'$ . It suffices to find a relation $C=\{c_1,c_2,\dots,c_m\}$ such that $\sum_{j=1}^m c_ja_{t-j}=0$ for all $t < k$ (or it could be undefined when $t < m$) , and for $k$ we get the value $1$ .
- Define the addition between two relation as adding their elements accordingly. It's not difficult to see that $C_k=C_{k-1}+\Delta\times C$ satisfies the constraint.
- Finally, how do we find such $C$ ?
- Suppose we have also run into this case at position $k'$ with error $\Delta'$ and relation $C'$. Then $\{1,-c_1',-c_2',\dots,-c_{m'}'\}$ will have the value $\Delta'$ at position $k'+1$ and $0$ otherwise. By dividing the whole sequence by $\Delta'$ and adding $k - k'-1$ zeroes in front, we get the $C$ !
- Finally, by checking all the position $k'$ , we can find the shortest recurrence relation.

Here's a sample code of Berlekamp-Massey Algorithm under modular arithmetic:

```cpp
void add(ll &x,const ll y){x += y;x %= P;}
void BM(ll *a,int n,vector <ll> &C){
    C.clear();
    //C' that leads to the shortest C
    vector <ll> lstC;
    //k' and delta' in the description
    int k = 0;ll delta = 0;
    rep(i,1,n){
        ll val = 0;
        rep(j,0,sz(C) - 1){
            //C is the last recurrence relation we have
            add(val,a[i - 1 - j] * C[j]);
        }
        if((a[i] - val) % P == 0)continue;
        if(k == 0){//first non-zero element
            k = i,delta = a[i] - val;
            rep(j,1,i)C.pb(0);
            continue;
        }
        vector <ll> now = C;
        ll t = (a[i] - val) * inv(delta) % P;
        if(sz(C) < sz(lstC) + i - k)C.resize(sz(lstC) + i - k);
        //incrementing
        add(C[i - k - 1],t);
        rep(j,0,sz(lstC) - 1)add(C[i - k + j],-t * lstC[j]);
        if(sz(now) - i < sz(lstC) - k){
            k = i,delta = a[i] - val,lstC = now;
        }
    }
}
```

### How to find the k-th term of a sequence with the linear recurrence relation?

Suppose we have a magic function $G$ that maps a polynomial to a real number in this way:


$$
G(\sum_{i=0}^n c_ix^i)=\sum_{i=0}^nc_ia_i
$$


Notice that for the polynomial $f(x)=x^{m}-\sum_{i=1}^m c_ix^{m-i}$ ,  we have


$$
G(x^kf(x))=a_{k+m}-\sum_{i=1}^m c_ia_{m+k-i}=0
$$


By the definition of linear recurrence relation. Thus,


$$
G(g(x)f(x))=0
$$


for any polynomial $g(x)$ .


$$
a_k=G(x^k)=G(r(x))
$$


where $r(x)$ is the remainder of $x^k$ when divided by $f(x)$ . We can calculate $x^k$ in a binary-exponentiation manner then calculate $x^k\mod f$ . Time complexity will be $O(m^2\log k)$ or $O(m\log m\log k)$ if we speed up the polynomial multiplication part by using FFT.



 