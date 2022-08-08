Here's another way of determining $f$ (I think it's a lot easier than a1267ab's post)
$$
f(n)=\sum\binom{1+t_1+t_2+\dots+t_n}{1+t_1,t_2,\dots,t_n}
$$
Let $t_1'=1+t_1\ge 1$ , so we only need to find all n-tuples $(t_1',t_2,\dots,t_n)$ that satsisfies  $t_1'+\sum_{j>1}j\times t_j=n+1,t_1'\ge 1,t_2,\dots,t_n\ge 0$ . Notice that this is actually the number of solution with $t_1'\ge 0$ subtracted by the number of solution that $t_1'=0$ . By generating function we have
$$
\begin{aligned}
\sum f(n)x^{n+1}&=\sum (x+x^2+\cdots)^k-\sum (x^2+x^3+\cdots)^k\\
&=\sum(\frac{x}{1-x})^k-\sum(\frac{x^2}{1-x})^k\\
&=(1-\frac{x}{1-x})^{-1}-(1-\frac{x^2}{1-x})^{-1}\\
&=\frac{1-x}{1-2x}-\frac{1-x}{1-x-x^2}\\
&=(1+\frac{x}{1-2x})-(1+\frac{x^2}{1-x-x^2})\\
&=x(\frac{1}{1-2x}-\frac{x}{1-x-x^2})\\
\end{aligned}
$$
Now we know that $f(n)=2^n-F_n$ where $F_n$ is the $n$th fibonacci number $(F_1=1,F_2=1,F_{n+2}=F_{n+1}+F_n)$ It suffices to prove that
$$
\sum_{1\le i<j<k\le p-1}\frac{F_i}{ijk}\equiv 0\pmod{p}
$$
