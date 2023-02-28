### Group action

*Definition:* If $G$ is a group with identity element $e$, and $X$ is a set, then a group action $\alpha$ of $G$ on $X$ is a function


$$
\alpha: G\times X\to X
$$


that satisfies two axioms for all $g,h\in G,x\in X$:

- identity: $\alpha(e,x)= x$
- compatibility $\alpha(g,\alpha(h,x))=\alpha(gh,x)$

with $\alpha(g,x)$ often shortened to $gx$ or $g\cdot x$ when there's no ambiguity of the action being considered in the context. The group $G$ is said to act on $X$; a set $X$ together with the action of $G$ is called a $G$-set.

From these two axioms, we know that the function that maps $x$ to $g\cdot x$ is a bijection, since we have the inverse map $g^{-1}$ such that $g^{-1}\cdot (g\cdot x)=x$.

### Orbits and stabilizers

Consider a group $G$ acting on a set $X$.

*Definition:* The *orbit* of an element $x\in X$ is the set of elements in $X$ which $x$ can be moved to through the group action, denoted by $G\cdot x$:


$$
G\cdot x=\{g\cdot x|g\in G\}
$$


*Proposition:* If and only if there exists a $g\in G$ such that $g\cdot x =y$ for $x,y\in X$, we say that $x\sim y$. This is an equivalence relation.

*Proof:*

- Reflectivity: $e\cdot x = x\implies x\sim x $
- Symmetry: $x\sim y\implies g\cdot x = y\implies x=g^{-1}\cdot y\implies y\sim x$
- Transitivity: $x\sim y\land y\sim z\implies g\cdot x = y,h\cdot y=z\implies h\cdot(g\cdot x) =(hg)\cdot x=z\implies x\sim z$

This proposition implies that the set of orbits of $X$ under the group action $G$ forms a partition of $X$.

*Definition:* Given $g\in G$ and $x\in X$ with $g\cdot x =x$, we say $x$ is fixed under the action of $g$. For every $x\in X$, its *stabilizer* in $G$ is the set of all elements $g\in G$ that fixes $x$, denoted as $G_x$:


$$
G_x=\{g\in G|g\cdot x = x\}
$$


The *stabilizer* is more often called a ***stabilizer subgroup***! Indeed, we can prove this by verifying the conditions:

For $g\in G_x$, we have 


$$
x=e\cdot x=(g^{-1}g)\cdot x =g^{-1}\cdot(g\cdot x)=g^{-1}\cdot x
$$


Thus $g^{-1}\in G_x$.



The identity element $e$ is clearly in $G_x$ since $e\cdot x =x$.

For $g,h\in G_x$,



$$
gh\cdot x =g\cdot(h\cdot x)=g\cdot x =x\implies gh\in G_x
$$


Thus, $G_x$ is a subgroup of $G$.



### Orbit-stabilizer theorem

*Theorem:* For a finite group $G$ acting on a set $X$ and any element $x\in X$. 


$$
|G\cdot x| = [G:G_x] =\frac{|G|}{|G_x|}
$$


*Proof:* For a fixed $x\in X$, consider the map $f:G\to X$ given by mapping $g$ to $g\cdot x$. By definition, the image of $f(G)$ is the orbit of $G\cdot x$. If two elements $g,h\in G$ have the same image:


$$
f(g)=f(h)\Longleftrightarrow g\cdot x =h\cdot x\Longleftrightarrow g^{-1}h\cdot x = x\Longleftrightarrow g^{-1}h\in G_x\Longleftrightarrow h\in gG_x
$$


In other words, $f(g)=f(h)$ if and only if they are in the same coset for the *stabilizer subgroup* $G_x$. Hence, $f$ induces a bijection between the set $G/G_x$ of cosets and the orbit $G\cdot x$ which sends $gG_x\mapsto g\cdot x$. Combining the bijection with lagrange theorem gives *orbit-stabilizer theorem* as desired.

### Burnside's Lemma

This is a lemma useful in taking account of symmetry when counting mathematical objects. It can be proven by *orbit-stabilizer theorem* and we will show that in later sections.

*Theorem:* Let $G$ be a finite group acting on a set $X$. For each $g\in G$, let $X^g$ denote the set of elements in $X$ that are fixed by $g$:


$$
X^g=\{x\in X|g\cdot x = x\}
$$


Burnside's lemma provides a formula for the number of orbits, denoted by $X/G$:


$$
|X/G| = \frac{1}{|G|}\sum_{g\in G}|X^g|
$$


#### Examples

First, we want to see how we can apply it in real life. Suppose we want to see how many ways we can color a cube with 3 colors, it would be simply $3^6=729$ ways. However, what if two colorings are essentially the same after some rotations?

Let $X$ be the set of all $3^6$ color combination and $G$ be the rotational symmtery group of the cube acting on $X$ in the natural manner. There are $6\times 4= 24$  elements in $G$.

Then two elements of $X$ belong to the same orbit if one can be rotated to the other. Hence, we are finding the number of orbits! Now, it suffices to calculate $X^g$ for all $g\in G$:

- the identity element fixes all elements of $X$.
- six 90-degree face rotations, each fixes $3^3$ elements of $X$.
- three 180-degree face rotations, each fixes $3^4$ elements of $X$.
- eight vertex rotations, each fixes $3^2$ elements of $X$.
- six edge flip, each fixes $3^3$ elements of $X$.

Hence, the average fix size is


$$
\frac1{24}(3^6+6\times 3^3+3\times 3^4+8\times 3^2+6\times 3^3)=57
$$


Finally, how do we prove this lemma?

*Proof:* First, we would like to re-express the sum over group elements $g\in G$ as an equivalent sum over elements $x\in X$:


$$
\sum_{g\in G}|X^g|=|\{(g,x)\in G\times X|g\cdot x = x\}|=\sum_{x\in X}|G_x|
$$


By *orbit-stabilizer theorem*, we can rewrite the equation into:


$$
\sum_{x\in X}|G_x|=|G|\sum_{x\in X}\frac{1}{|G\cdot x|}
$$


Finally, notice that the set of orbits partition $X$, so we can break the sum into separate sums over each individual orbit:


$$
\sum_{x\in X}\frac{1}{|G\cdot x|}=\sum_{A\in X/G}\sum_{x\in A}\frac{1}{|A|}=|X/G|
$$


Putting everything together, we get:


$$
|X/G| = \frac{1}{|G|}\sum_{g\in G}|X^g|
$$
