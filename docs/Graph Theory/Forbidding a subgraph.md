An interesting question that mathematicians are curious about is the maximum number of edges in a n-vertex graph $G$ that forbids some other graph $H$. In other words, $G$ cannot contain $H$ as a subgraph.

We may also start from the simplest question, **what is the maximum number of edges in a n-vertex graph without any triangle?** This is a foundational problem in extremal graph theory, where people study how the global properties of a graph influence local substructure.

> Hint: Try solving the case when $n=3,4,5$. Do you see any pattern?

*Mantel's Theorem:* $K_{\lfloor n/2\rfloor,\lceil n/2\rceil}$ has the most number of edges among all triangle-free graphs. So every $n$-vertex triangle-free graph has at most $\lfloor n^2/4\rfloor$ edges.

We will give two different proofs of Mantel's theorem and each illustrates a different technique in combinatorics. The proof requires some knowledge of [Cauchy-Schwarz Inequality](https://en.wikipedia.org/wiki/Cauchy%E2%80%93Schwarz_inequality), which states that for any two list of real numbers $a_1,a_2,\dots,a_n$ and $b_1,b_2,\dots,b_n$, we have:


$$
(a_1^2+a_2^2+\cdots+a_n^2)(b_1^2+b_2^2+\cdots+b_n^2)\ge(a_1b_1+a_2b_2+\cdots+a_nb_n)^2
$$


It has many proofs but I will not show them here since it's unrelated to our topics. Feel free to read one of my previous blog post on [Olympiad Inequalities](https://yifank.github.io/Math/Olympiad%20Inequalities/), in which I discusses more about all these inequalities.





