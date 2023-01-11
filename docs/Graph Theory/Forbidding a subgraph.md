An interesting question that mathematicians are curious about is the maximum number of edges in a n-vertex graph $G$ that forbids some other graph $H$. In other words, $G$ cannot contain $H$ as a subgraph.

We may also start from the simplest question, **what is the maximum number of edges in a n-vertex graph without any triangle?** This is a foundational problem in extremal graph theory, where people study how the global properties of a graph influence local substructure.

> Hint: Try solving the case when $n=3,4,5$. Do you see any pattern?

*Mantel's Theorem:* $K_{\lfloor n/2\rfloor,\lceil n/2\rceil}$ has the most number of edges among all triangle-free graphs. So every $n$-vertex triangle-free graph has at most $\lfloor n^2/4\rfloor$ edges.

We will give two different proofs of Mantel's theorem and each illustrates a different technique in combinatorics.







