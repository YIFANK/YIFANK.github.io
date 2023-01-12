In a problem with $n$ variables, $\sum_{cyc}$ means to cycle through $n$ variables, and $\sum_{sym}$ means to go through all the $n!$ permutations. e.g.
$$
\sum_{cyc}a^2=a^2+b^2+c^2\\
\sum_{sym}a^2=a^2+a^2+b^2+b^2+c^2+c^2\\
\sum_{cyc}ab=ab+bc+ca\\
\sum_{sym}a^2b=a^2b+ab^2+a^2c+ac^2+b^2c+bc^2\
$$

### Basic Inequalities

> *(AM-GM Inequality)* For non-negative real numbers $a_1,a_2,\dots,a_n$, we have:
> $$
> \frac{a_1+a_2 + \cdots+a_n}{n}\ge \sqrt[n]{a_1a_2\cdots a _n}
> $$

*Proof:* We apply a beautiful method called *Cauchy Induction*:

- First, we show that the statement holds when $n=2$:

$$
\frac{a_1+a_2}{2}\ge\sqrt{a_1a_2}\Leftrightarrow (a_1+a_2)^2\ge 4a_1a_2\Leftrightarrow (a_1-a_2)^2\ge 0
$$

- Next, we show that if the statement holds for $n$, then it holds for $2n$:

Notice that
$$
\frac{a_1+a_2+\cdots +a_{2n}}{2n}=\frac{\frac{a_1+a_2+\cdots+a_n}{n}+\frac{a_{n+1}+a_{n+2}+\cdots+a_{2n}}{n}}{2}
$$
By the previous cases:
$$
\frac{a_1+a_2+\cdots +a_{2n}}{2n}\ge\frac{\sqrt[n]{a_1a_2\cdots a_n}+\sqrt[n]{a_{n+1}a_{n+2}\cdots a_{2n}}}{2}\ge\sqrt{\sqrt[n]{a_1a_2\cdots a_n}\times\sqrt[n]{a_{n+1}a_{n+2}\cdots a_{2n}}}
$$
Thus
$$
\frac{a_1+a_2+\cdots +a_{2n}}{2n}\ge \sqrt[2n]{a_1a_2\cdots a_{2n}}
$$

- Finally, we show that the statement holds for $n$ implies that it holds for $n-1$:

Let $a_n = \frac{a_1+a_2+\cdots + a_{n-1}}{n-1}$:
$$
\frac{\sum_{i=1}^n a_i}{n}=\frac{\sum_{i=1}^{n-1}a_i}{n-1}
$$
Thus:
$$
\frac{\sum_{i=1}^{n-1}a_i}{n-1}\ge\sqrt[n]{a_1a_2\cdots a_n}\\
\bigg(\frac{\sum_{i=1}^{n-1}a_i}{n-1}\bigg)^n\ge \prod_{i=1}^{n-1}a_i\times\frac{\sum_{i=1}^{n-1}a_i}{n-1}\\
\bigg(\frac{\sum_{i=1}^{n-1}a_i}{n-1}\bigg)^{n-1}\ge \prod_{i=1}^{n-1}a_i\\
\frac{a_1+a_2+\cdots + a_{n-1}}{n-1}\ge \sqrt[n-1]{a_1a_2\cdots a_{n-1}}
$$

> Example: Prove that $a^2+b^2+c^2\ge ab+bc+ca,a^4+b^4+c^4\ge a^2bc+ab^2c+abc^2$

*Proof:* By AM-GM,
$$
a^2+b^2+c^2=\sum_{cyc}\frac{a^2+b^2}2\ge \sum_{cyc}ab\\
a^4+b^4+c^4=\sum_{cyc}\frac{a^4+a^4+b^4+c^4}{4}\ge \sum_{cyc}\sqrt[4]{a^8b^4c^4}=\sum_{cyc} a^2bc
$$

> Example: $a^3+b^3+c^3\ge a^2b+b^2c+c^2a$

The fundamental intuition is being able to decide which symmetric polynomials of a given degree are bigger. A useful formalization to all the patterns above is Muirhead's inequality.

> *(Muirhead's Inequality)* Suppose we have two real sequences $x_1\ge x_2\ge\cdots\ge x_n,y_1\ge y_2\ge\cdots\ge y_n$ of length $n$, such that:
> $$
> x_1+x_2+\cdots+x_n=y_1+y_2+\cdots+y_n\\
> x_1+x_2+\cdots+x_k\ge y_1+y_2+\cdots+y_k\quad\text{for all }k < n
> $$
> We say that $(x_n)$ majorizes $(y_n)$, which is often written as $(x_n)\succ(y_n)$ . Then
> $$
> \sum_{sym}a_1^{x_1}a_2^{x_2}\cdots a_n^{x_n}\ge \sum_{sym}a_1^{y_1}a_2^{y_2}\cdots a_n^{y_n}
> $$

It is not difficult to show this by AM-GM so the whole proof is left to the readers.

> *(Cauchy-Schwarz Inequality)* For any list of real numbers $(a_n),(b_n)$, we have
> $$
> (a_1^2+a_2^2+\cdots +a_n^2)(b_1^2+b_2^2+\cdots b_n^2)\ge (a_1b_1+a_2b_2+\cdots+a_nb_n)^2
> $$
> The equality holds when we have two constants $\mu,\lambda$ not all zero such that $\mu a_i=\lambda b_i$ for any $1\le i\le n$.

*Proof:* There are many proofs to this inequality. We provide a prove by constructing a quadratic polynomial:
$$
0\le (a_1x+b_1)^2+(a_2x+b_2)^2+\cdots(a_nx+b_n)^2=\sum_{i=1}^n a_i^2x^2+2\sum_{i=1}^na_ib_i x + \sum_{i=1}^nb_i^2
$$
Since it's nonnegative, it has at most one solution so its determinant must be no greater than zero:
$$
(\sum_{i= 1}^n a_ib_i)^2 -(\sum_{i=1}^n a_i^2)(\sum_{i=1}^nb_i^2)\le 0
$$
There is also a complex form of this inequality, for complex number sequences $(a_n),(b_n)$, we have
$$
(|a_1|^2+|a_2|^2+\cdots +|a_n|^2)(|b_1|^2+|b_2|^2+\cdots |b_n|^2)\ge |a_1b_1+a_2b_2+\cdots +a_nb_n|^2
$$

### Inequalities in functions

We say a function $f$ is *convex* if $f''(x)\ge 0$ for all $x$ and *concave* otherwise.

> *(Jensen's Inequality)* Suppose $f$ is convex, then
> $$
> \frac{f(a_1)+f(a_2)+\cdots +f(a_n)}{n}\ge f(\frac{a_1+a_2+\cdots + a_n}{n})
> $$

By convexity, we have that for all non-negative $\lambda_1+\lambda_2 = 1$:
$$
f(\lambda_1 a_1+\lambda_2a_2)\le \lambda_1f(a_1) +\lambda_2f(a_2)
$$
It can be generalized to $n$ non-negative numbers such that $\lambda_1+\lambda_2+\cdots +\lambda_n = 1$, we have:
$$
f(\sum_{i=1}^n\lambda_ia_i)\le\sum_{i=1}^n\lambda_if(a_i)
$$
Intuitively, this is telling you that any point in the region bounded by $(a_i,f(a_i))$ is above the function. Pick all $\lambda_i=\frac{1}n$ and we have *Jensen's inequality*.

> *(Karamata's Inequality)* If $f$ is convex and $(x_n)\succ (y_n)$, then
> $$
> \sum_{i=1}^n f(x_n)\ge \sum_{i=1}^n f(y_n)
> $$

*Proof:* Define $c_i$ as the slope of the line through $(x_i,f(x_i)),(y_i,f(y_i))$, then by convexity:
$$
c_{i+1}=\frac{f(x_{i+1})-f(y_{i+1})}{x_{i+1}-y_{i+1}}\le \frac{f(x_i)-f(y_i)}{x_i-y_i}=c_i
$$
Define $(A_n),(B_n)$ the prefix sum of $(x_n),(y_n)$ respectively, so $A_0=B_0=0$ and $A_i=\sum_{i=1}^n x_i,B_i=\sum_{i=1}^n y_i$. Then:
$$
\begin{aligned}
\sum_{i=1}^n(f(x_i)-f(y_i)) &=\sum_{i=1}^n c_i(x_i-y_i)\\
&= \sum_{i=1}^n c_i(A_i-A_{i-1}-(B_i-B_{i-1}))\\
&= \sum_{i=1}^n c_i(A_i-B_i)-\sum_{i=1}^nc_i(A_{i-1}-B_{i-1})\\
&=c_n(A_n-B_n)+\sum_{i=1}^n(c_i-c_{i+1})(A_i-B_i) - c_1(A_0-B_0)\\
&= \sum_{i=1}^n(c_i-c_{i+1})(A_i-B_i)\ge 0\\
\end{aligned}
$$
We can also attain Jensen's Inequality directly from this by taking $(a_1,a_2,\dots,0)\succ (\frac{a_1+a_2+\cdots+a_n}{n},\frac{a_1+a_2+\cdots+a_n}{n},\cdots,\frac{a_1+a_2+\cdots+a_n}{n})$ , where $a_1\ge a_2\ge\cdots\ge a_n$ .



