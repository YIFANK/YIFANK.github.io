Recently when I was solving some random math olympiad problems, I found this interesting theorem and decide to share it.

Given two sets $A,B$ , define


$$
A+B=\{a+b|a\in A,b\in B\}
$$


If the two sets are defined over real numbers, we can prove that


$$
|A+B|\ge |A|+|B|-1
$$


It is not difficult to show that this is true, as we are able to order the elements in $A$ and $B$ as $a_1<a_2<\cdots<a_n,b_1<b_2<\cdots<b_m$ . Then


$$
a_1+b_1<a_1+b_2<\cdots<a_1+b_m<a_2+b_m<\cdots<a_n+b_m
$$


Thus, there are at least $n+m-1$ elements in $A+B$ ! The equality holds only when $A,B$ are arithmetic sequences with the same common difference. The original theorem states the inequality when $A,B$ are in a cyclic group with prime order $\mathbb{Z}/{p\mathbb{Z}}$ .


$$
|A+B|\ge \min\{p,|A|+|B|-1\}
$$


> Lemma 1: If $G$ is a finite abelian group and $A,B$ are nonempty subsets of $G$ such that $|A|+|B|> |G|$, then $A+B=G$ .

Proof: This is equivalent to proving that for any $g\in G$ , there exist $a\in A,b\in B$ such that $a+b=g$. Define $g-B=\{g-b|b\in B\}$ , then $|g-B|=|B|$ . We now have $|A|+|g-B|>|G|$ . Thus $A\cap g-B\ne\empty$ , pick $a\in A\cap g-B$ , we get a solution for $a+b=g$ .







