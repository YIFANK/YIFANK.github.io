> *(Cauchy's Theorem)* For a finite group $G$ and a prime number $p$ that divides the order of $G$ (the number of element in $G$), then there exists an element of order $p$ in $G$. That is, an element $x$ such that the smallest positive integer $n$ with $x^n=e$ is $p$.

*Proof:*

We define a set 


$$
S=\{(x_1,\dots,x_p)\in G^p|x_1x_2\cdots x_p=e\}
$$


We see that any element in $S$ is uniquely determined by the first $p-1$ components, since


$$
x_p=(x_1x_2\cdots x_{p-1})^{-1}
$$


Thus,


$$
|S|=|G|\times |G|\times\cdots\times |G|=|G|^{p-1}
$$


Moreover, notice that $aa^{-1}=a^{-1}a=e$ for all $a\in G$. For $(x_1,x_2,\dots,x_p)$ in $S$,


$$
x_2x_3\cdots x_p x_1=x_1^{-1}x_1=e\implies (x_2,x_3,\dots,x_p,x_1)\in S
$$


We can see this as cyclicly permuting the components of $x\in S$. By the orbit-stabilizer Theorem, orbits in $S$ under this action have size $1$ or $p$. The former one only occurs for tuples $(x,x,\dots,x)\in S$ such that $x^p=e$.

We count the number of elements of $S$ by orbits, then we know that the number of $x\in G$ such that $x^p=e$ has to be divisble by $p$. Since $x=e$ is one such element, we know that there must be at least $p-1$ other elements as solutions for $x$. They all have order $p$ and this completes the proof.

> *(Sylow Theorems)*
>
> 1, A finite group $G$ whose order $|G|$ is divisible by $p^k$ has a subgroup of order $p^k$.

We can apply a similar idea to prove this theorem.

*Proof:*

Let $|G|=p^km=p^{k+r}u$ such that $p\nmid u$. Let $S$ denote the set of subsets of $G$ with size $p^k$. $G$ acts on $S$ by left multiplication, for $g\in G$ and $s\in S$, 


$$
g\cdot s=\{gx|x\in s\}
$$


For a given set $s\in S$, define its stabilizer subgroup 


$$
G_s=\{g\in G|g\cdot s = s\}
$$


and its orbit


$$
Gs=\{g\cdot s|g\in G\}
$$


It suffices to show that there exist some $s$ such that $|G_s|=p^k$ since $G_s$ is a subgroup of $G$. This is the maximal possible size of $G_s$, since for any fixed element $\alpha\in s\subseteq G$, the right coset $G_s\alpha$ is a subset of $s$. Hence

$$
|G_s|=|G_s\alpha|\le |s|=p^k
$$


By the orbit-stabilizer theorem, we have

$$
|G_s||Gs| = |G|=p^{k+r}u
$$

for all $s\in S$. Define $v_p(n)$ as the largest non-negative integer $k$ such that $p^k|n$. If we count the number of factors of $p$ on both sides, we get


$$
v_p(|G_s|) + v_p(|Gs|) = v_p(|G|)=k+ r
$$


This implies that when $v_p(|G_s|)< k$ we have $v_p(|Gs|) > k+r-k=r$. Notice that in order to have $v_p(|G_s|)=k$, one would have $|G_s|=p^k$, which are the ones we are looking for.

Now we count the number of elements in $S$ in two ways. By basic combinatorics, we know that


$$
|S|=\binom{p^km}{p^k}
$$


and $|S|$ is also the sum of $|Gs|$ over all distinct orbits $Gs$. By Kummer's theorem we get


$$
v_p(|S|)=r
$$


If we don't have any $s$ such that $|G_s|=p^k$, then all $s\in S$ have orbit $Gs$ with $v_p(|Gs|) > r$. The sum of them contradicts with $v_p(|S|)=r$, completing the proof!

