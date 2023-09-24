Suppose Alice and Bob are playing a game with a pile with $n$ coins and a pile with $m$ coins. Of course, $n$ and $m$ are non-negative integers. They take turns removing coins in any of the following ways:

- Take a positive number of coins from pile 1.
- Take a positive number of coins from pile 2.
- Take the same positive number of coins from pile 1 and 2.

The player that takes the last coin will win the game. Determine all $(n,m)$ such that Alice will win. This is an interesting mathematical game called **Wythoff's game**.

> *Definition:* We call $(n,m)$ a winning position if Alice can win no matter what Bob do. Otherwise, $(n,m)$ is a losing position, which means that if Bob plays optimally Alice will lose.

> *Proposition*: $(n,m)$ is a winning position if there exist an operation for Alice to turn it to a losing position. Similarly, $(n,m)$ is a losing position if it's impossible for Alice to move to a losing position.

The proposition is trivial by definition, since after the operation, Bob will have a losing position when it's his turn to move, resulting in Alice winning.

> *Lemma:* If $(n,m)$ is a losing position, then
>
> - $(n+k,m)$ is a winning position for all $k\in\mathbb Z^+$.
> - $(n,m+k)$ is a winning position for all $k\in\mathbb Z^+$.
> - $(n+k,m+k)$ is a winning position for all $k\in\mathbb Z^+$.

Now, we are curious of all the losing positions. First, $(0,0)$ is a losing position since Alice cannot make a move. First, we can use brute force to find some losing positions.
$$
(0,0),(1,2),(3,5),(4,7),(6,10),\dots
$$
(By symmetry, we only care about the cases when $n \le m$)

We want to find a formula to describe the sequence of all losing positions. We can find that by describing our brute force strategy rigorously.

Let $(a_0,b_0)=(0,0)$. By our lemma, there does not exist $i\neq j$ such that $a_i=b_j$ or $a_i-b_i=a_j-b_j$. Hence, all $a_i,b_i$ are distinct and all values of $a_i-b_i$ are distinct.

We can infer that $a_i < b_i$ for $i > 0$. We find $(a_{n+1},b_{n+1})$ to be the next smallest possible losing position after the first $n$ values. 

For $n\in\mathbb Z^+$, let $S_n=\{a_k|0\le k < n\}\cup\{b_k|0\le k < n\}$. Then by our definition,
$$
a_n = \text{mex}(S_n),b_n=a_n+n
$$
Then $(a_n,b_n)$ contains all losing positions $(n,m)$ with $n\le m$.

Finally, we assert that $a_n = \lfloor n\phi\rfloor,b_n=\lfloor n\phi^2\rfloor$, where $\phi=\dfrac{1+\sqrt5}{2}$.

We can verify the assertion:
$$
\begin{aligned}
b_n-a_n &=\lfloor n\phi^2\rfloor-\lfloor n\phi\rfloor\\
&=\lfloor n(\phi+1)\rfloor-\lfloor n\phi\rfloor\\
&=n
\end{aligned}
$$
and since $1/\phi + 1/\phi^2=1$, the sequence $(a_n),(b_n)$ with $n > 0$ partition positive integers by [Rayleigh's theorem](https://en.wikipedia.org/wiki/Beatty_sequence). The proof of this theorem is left to the readers.