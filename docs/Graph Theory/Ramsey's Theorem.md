From now on, I will be creating a series of blog posts as well as presentations on graph theory. The blog post is inspired by Prof. Yufei Zhao's book Graph Theory and Additive Combinatorics. I will be discussing selected topics and problems in this book as well as providing solutions to some exercises.

So, what is graph theory?

I am bad at explaining things, so I asked chatGPT for its definition:

> ChatGPT: "Graph theory is a branch of mathematics that studies the properties and structures of graphs, which are mathematical representations of networks of objects or relationships between them. Graphs can be used to model a wide range of problems in various fields, including computer science, engineering, social sciences, biology, physics, and more. Some of the basic concepts in graph theory include vertices (or nodes), edges, and various types of graph such as directed, undirected, weighted and unweighted."

First, we introduce some notations to define a graph.

We write a graph as $G=(V,E)$, where $V$ is the set of vertices and $E\subseteq \{(x,y)|x,y\in V\}$ is the set of edges.

For example, when $V=\{1,2,3,4\},E=\{(1,3),(1,4),(2,3),(2,4)\}$. The graph looks like this:

![img](https://raw.githubusercontent.com/YIFANK/YIFANK.github.io/main/pics/image-20221207100910624.png)

This is also known as a $K_{2,2}$ graph, which means we have two vertices on the left and other two on the right, and every vertex on the left is connected with every vertex on the right.

Generally, we define **the complete bipartite graph** $K_{s,t}$ to be a graph with $s$ vertices in one part and $t$ on the other, and any edges in between the two parts are connected.

Moreover, $K_r$ is the **complete graph** on $r$ vertices, which means $E=\{(x,y)|1\le x < y\le r \}$ . It is also known as a $r$-clique.

We can describe many interesting problems using these definition and we will try solving some of them.

> *(Multicolor Triangle Ramsey's Theorem)* For every positive integer $r$, there is some integer $N=N_r$ such that if each edge of $K_N$ is colored using one of the $r$ colors, then there will be a monochromatic (one color) triangle.

First, it's clear that if $r=1$, then $N_r=3$ simply satisfy the condition since there is only one color. We want to prove a non-trivial case when $r=2$. When $N=5$, we can give a construction where there is no monochromatic triangle like this:

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/RamseyTheory_K5_no_mono_K3.svg/210px-RamseyTheory_K5_no_mono_K3.svg.png)

However, $N_2=6$, which means that no matter how we color $K_6$, there will be a triangle with edges sharing the same color! If we see people as vertices and mutual friendship as the edges of a graph, this theorem tells us that among any $6$ people, there must be a group of $3$ that are friends of one another or that are complete strangers.

Interesting! But how do we prove this? First, we need to know pigeonhole principle:

> *(Pigeonhole Principle)* If $n=km+t$ items $(k,t>0)$ are put into $m$ containers, at least one of the containers will contain $k+1$ items.

*Proof:* Suppose the $i$-th container contain $c_i$ items and all containers contain less than $k+1$ items, then:


$$
n=c_1+c_2+\cdots+c_m\le k+k+k+\cdots+k=km<km+t=n
$$


Contradiction!

It is not hard to further show that the maximum value of $k+1$ in this theorem is $\lceil n/m\rceil$ . Intuitively, the principle tells us that the larger number of items we have, there will be a container with more items in it.

Back to the original problem. For the $5$ edges that have vertex $1$, by pigeonhole principle, at least $3$ of them will have the same color! Since the labelings don't matter, let's say the three edges are $(1,2),(1,3)$ and $(1,4)$ are colored red. Now:

- If $(2,3)$ is red. We have $(1,2),(1,3),(2,3)$ a red triangle. We can get similar red triangles if either $(2,4)$ or $(3,4)$ is red.
- If none of $(2,3),(2,4),(3,4)$ is red, then they must be all blue. $(2,3),(2,4),(3,4)$ indeed forms a all blue triangle!

Thus, in all cases we have found a monochromatic triangle, and $N(2)=6$ is proved! Now we can move on to the more generalized case and try proving it with the same idea and induction on $r$. 

Suppose the claim is true for $r-1$ colors, which means that we can find a monochromatic triangle in any $r-1$ colored complete graph of $N_{r-1}$ vertices. It is left to somehow reduce a larger graph into a case like that, then we are done.

Notice how we reduce the $N=6,r=2$ case to $N=3,r=1$ by pigeonhole? We can do it again! If we take $N$ large enough, there will be more than $N_{r-1}$ edges of the same color coming out from vertex $1$. Then for the $N_{r-1}$ other vertices in these edges, the edges between them cannot be of the same color, which reduces the coloring among them into $r-1$ colors. Finally, we can get a monochromatic triangle in them following by our induction. Numerically, the large enough $N$ is $N_r=r(N_{r-1}-1)+2$. The readers can verify that this suffices for our pigeonhole to work.

There are some more variations of this theorem, which can be proven by applying the pigeonhole principle inductively in a similar style as above.

- *(Ramsey's Theorem)* For every $k$ and $r$ there exist some integer $N=N(k,r)$ such that if each edge of $K_N$ is colored using one of the $r$ colors, then there is a monochromatic $K_k$.
- *(Ramsey's Number)* For every $r$ and $b$ there exist an integer $n=R(r,b)$ such that if each edge of $K_N$ is colored either blue or red, either a red monochromatic subgraph $K_r$ or a blue monochromatic subgraph $K_b$ exists.

Though it is not difficult proving such integer exists, finding the exact Ramsey number is extremely hard and mathematicians have been working on this problem for decades! In fact, people still haven't found the value of $R(5,5)$ though it has been proven to be no less than 43 (Geoffrey Exoo (1989)) and no greater than 48 (Angeltveit and McKay (2017)).

> *(Schur's Theorem)* For every positive integer $r$, there exists a possible integer $N=N(r)$ such that each element of $[N]$ is colored using oe of $r$ colors, then there is a monochromatic solution to $x+y=z$ ($x,y,z$ have the same color).

*Proof:* Assume we have the coloring $\phi$, for $[N]$, then for each edge of a complete graph we color the edge $(i,j)$ with the color $\phi(j-i)$ when $i<j$.

By Ramsey's Theorem, there is a monochromatic triangle, so there exist $i<j<k$ such that



$$
\phi(j-i)=\phi(k-j)=\phi(k-i)
$$



Take $x = j-i,y=k-j,z=k-i$, then $\phi(x)=\phi(y)=\phi(z)$ and $x+y=z$. 

> "Now that we proved Schur’s theorem, let us pause and think about what did we gain by translating the problem to graph theory? We were able to apply Ramsey’s theorem, whose proof considers restrictions to subgraphs, which would have been rather unnatural if we had worked exclusively in the integers. Graphs gave us greater flexibility." - Yufei Zhao











