**JMO 1.** Let $\mathbb{N}$ denote the set of positive integers. Find all functions $f:\mathbb{N}\rightarrow\mathbb{N}$ such that for all positive integers $a$ and $b$,


$$
f(a^2+b^2)=f(a)\cdot f(b) \qquad \text{and}\qquad f(a^2)=f(a)^2
$$


Solution: 

First, we have


$$
f(1^2)=f(1)^2\Longrightarrow f(1)^2-f(1)=f(1)(f(1)-1)=0
$$


Since $f(1)$ is a positive integer, $f(1)=1$.


$$
f(2)=f(1^2+1^2)=f(1)\cdot f(1)=1
$$


We could make a conjecture that $f(n)=1$ for all $n\in\mathbb{N}$. Next we will prove it by induction. Notice that $(a^2+b^2)^2=(a^2-b^2)^2+(2ab)^2$, then:


$$
\begin{aligned}
f((a^2+b^2)^2)=f((a^2-b^2)^2+(2ab)^2)=f(a^2-b^2)\cdot f(2ab)\\
f((a^2+b^2)^2)=f(a^2+b^2)^2=(f(a)\cdot f(b))^2=f(a)^2\cdot f(b)^2\\
\Longrightarrow f(a^2-b^2)\cdot f(2ab)=f(a)^2\cdot f(b)^2 \qquad(*)
\end{aligned}
$$


If for all positive integers $x<n,f(x)=1$. Then

- If $n=2k+1,k\in\mathbb{N}$. Take $a=k+1,b=k$ in the equation $(*)$,


$$
\begin{aligned}
f((k+1)^2-k^2)\cdot f(2(k+1)k)=f(2k+1)\cdot f(2k^2+2k)=f(k+1)^2\cdot f(k)^2
\end{aligned}
$$



Since $k<n$ and $k+1<n$ ,


$$
\begin{aligned}
\Longrightarrow f(k+1)^2\cdot f(k)^2=1\\
\Longrightarrow f(2k+1)\cdot f(2k^2+2k)=1
\end{aligned}
$$


By $f(x)\ge1$, it is clear that $f(n)=f(2k+1)=1$.



- If $n=2k,k\in\mathbb{N}$. Take $a=k,b=1$ in the equation $(*)$,


$$
\begin{aligned}
f(a^2-b^2)\cdot f(2ab)=f(k^2-1)\cdot f(2k)=f(k)^2\cdot f(1)^2=1\\
\Longrightarrow f(n)=f(2k)=1
\end{aligned}
$$



Thus, by induction, $f(n)=1$ for all $n\in\mathbb{N}$.

**JMO 2.** Rectangles $BCC_1B_2,CAA_1C_2,$ and $ABB_1A_2$ are erected outside an acute triangle $ABC$. Suppose that


$$
\angle{BC_1C}+\angle{CA_1A}+\angle{AB_1B}=180^\circ
$$


Prove that lines $B_1C_2,C_1A_2$ and $A_1B_2$ are concurrent.

Solution: 

Draw the circumcircles of the three rectangles. By some simple angle chasing the original conditon implies that the circles concur at a point $P$. Then $\angle{CPB_2}+\angle{CPA_1}=\angle{CBB_2}+\angle{CAA_1}=180^\circ$. Hence $P$ lies on $A_1B_2$ etc.

$\square$

**JMO 3.**An equilateral triangle $\Delta$ of side length $L>0$ is given. Suppose that $n$ equilateral triangles with side length 1 and with non-overlapping interiors are drawn inside $\Delta$, such that each unit equilateral triangle has sides parallel to $\Delta$, but with opposite orientation.


$$
n \leq \frac{2}{3} L^{2}
$$


Unfortunately I didn't solve this one during the contest. The main idea is finding the relation between this inequality and the area of all $\Delta$ . We could do this by drawing a regular hexagon of side length $1/2$ above every $\Delta$ , then prove all the hexagons are disjoint by doing casework. (the approach of Andrew Gu)

**JMO 4. **Carina has three pins, labeled $A, B$, and $C$, respectively, located at the origin of the coordinate plane. In a *move*, Carina may move a pin to an adjacent lattice point at distance $1$ away. What is the least number of moves that Carina can make in order for triangle $ABC$ to have area $2021$?

(A lattice point is a point $(x, y)$ in the coordinate plane where $x$ and $y$ are both integers, not necessarily positive.)

Notice that moving $ABC$ by the same vector won't change its area. Denote $A(x_A,y_A),B(x_B,y_B),C(x_C,y_C)$. Let $n$ be the number of moves. When $n$ is minimum, we have:


$$
\sum_{cyc}(|x_A|+|y_A|)\le\sum_{cyc}(|x_A-1|+|y_A|)
$$


Which means there are more non-negative $x$ than negative ones. Similarly, there are more non-positive $x$ than positive ones. Thus, at least one $x$ coordinate is $0$. Also at least one $y$ coordinate is $0$.

-  If $A(0,0)$ ,


$$
\begin{aligned}
2S_\triangle{ABC}&=|x_By_C-x_Cy_B|\\
&\le |x_B||y_C|+|x_C||y_B|\\
&\le \frac{1}{4}((|x_B|+|y_C|)^2+(|x_C|+|y_B|^2))\\
&\le \frac{1}{4}n^2\\
\end{aligned}
$$



Thus, $\dfrac{1}{4}n^2\ge2\cdot2021=4042$, which means $n\ge128$.

- If $A(x_A,0),B(0,y_B)$ , WLOG $x_A,y_B\ge 0, x_C,y_C\le 0$ .


$$
\begin{aligned}
2S_\triangle{ABC}&=|x_Ay_C+x_Cy_B+x_Cy_C|\\
&= (x_A+|x_C|)(y_B+|y_C|)-x_Ay_B\\
&\le \frac{1}{4}(x_A+|x_C|+y_B+|y_C|)^2\\
&\le \frac{1}{4}n^2\\
\end{aligned}
$$



This also leads to $n\ge 128$.

When $n=128$, $A(58,0),B(0,55),C(-6,-9)$ has area $2021$ .

Hence the answer is $\boxed{128}$ . 

**JMO 5.** A finite set $S$ of positive integers has the property that, for each $s \in S,$ and each positive integer divisor $d$ of $s$, there exists a unique element $t \in S$ satisfying $\text{gcd}(s, t) = d$. (The elements $s$ and $t$ could be equal.)

Given this information, find all possible values for the number of elements of $S$.

Solution:

The answer is $0,1$ and power of $2$. $S=\varnothing,\{1\}$ satisfy the condition obviously. Next, we want to show that for any $|S|\ge2$ , $|S|=2^k$.

1) For $|S|=2^k$ , pick $2k$ distinct prime numbers and denote them as $a_1,a_2,\dots,a_k$ and $b_1,b_2,\dots,b_k$. Let $S$ be the set of all $s=\prod_{i=1}^k p_i$ , $p_i=a_i$ or $p_i=b_i$ for each $i$. For each $s\in S$, **WLOG** $s=\prod_{i=1}^k a_i$ ( $b_i$ is another possible value of $p_i$ ). For


$$
\gcd(s,t)=d=\prod_{i=1}^{m}a_{x_i}
$$


For every position $x_i$ , $p_{x_i}=a_{x_i}$ for $t$ . And for the other positions $i$, $p_i=b_i$. $t\in S$ is clearly unique for every positive divisor $d$ .

**Lemma:** Let $D_s$ denote the set of positive divisors $s$ . For $s\in S$, $|D_s|=|S|$.

Proof: Since there exist a unique $t\in S$ for every $d\in D_s$ that $\gcd(s,t)=d$, and for every $t\in S$ we also have $\gcd(s,t)\in D_s$. This creates a bijection from $S$ to $D_s$, which means $|D_s|=|S|$.

$\square$

2) $|S|\neq2^k$ that satisfies the condition doesn't exist.

Let $s=\prod_{i=1}^k p_i^{\alpha_i}\in S$, then


$$
|D_s|=\prod_{i=1}^k (\alpha_i+1)=|S|
$$


There exist $\alpha_i>1$ or else $|S|$ will be a power of $2$. **WLOG**, $\alpha_1>1$, then let


$$
\begin{aligned}
\gcd(s,t)=d &=p_1^{\alpha_1-1}\prod_{i=2}^k p_i^{\alpha_i}\\
\end{aligned}
$$


Since $\gcd(s,\dfrac td)=1$ and consider  $|D_s|$ as a multiplicative function about $s$, $|D_t|$ is a multiple of $|D_d|=\alpha_1\prod_{i=2}^k(\alpha_i+1)$ . However, notice that since $t\in S$, $|D_t|=|S|=\prod_{i=1}^k (\alpha_i+1)$ . This means


$$
\alpha_1|\alpha_1+1\Longrightarrow \alpha_1=1
$$


**Contradiction!** 

Thus, the only possible value is $|S|=2^k$. 

**JMO 6.** Let $n \ge 4$ be an integer. Find all positive real solutions to the following system of $2n$ equations:


$$
\begin{aligned}
a_{1} &=\frac{1}{a_{2 n}}+\frac{1}{a_{2}}, & a_{2}&=a_{1}+a_{3}, \\
a_{3}&=\frac{1}{a_{2}}+\frac{1}{a_{4}}, & a_{4}&=a_{3}+a_{5}, \\
a_{5}&=\frac{1}{a_{4}}+\frac{1}{a_{6}}, & a_{6}&=a_{5}+a_{7} \\
&\vdots & &\vdots \\
a_{2 n-1}&=\frac{1}{a_{2 n-2}}+\frac{1}{a_{2 n}}, & a_{2 n}&=a_{2 n-1}+a_{1}
\end{aligned}
$$


Solution:

The answer is that the only solution is $a_{2i-1}=1,a_{2i}=2$ for $i=1,2,\dots,n$ which works. 

- $n=2k+1$, if there exists $a_{2i-1},a_{2i+1}\ge1$  for some positive integer $i$ , WLOG $a_1,a_3\ge1$ . Then


$$
\begin{aligned}
a_2&=a_1+a_3\ge2\\
\frac1{a_4}&=a_3-\frac1{a_2}\ge 1-\frac12=\frac12\Longrightarrow a_4\le2\\
a_5 &=a_4-a_3\le 2-1=1\\
\frac1{a_6}&=a_5-\frac1{a_4}\le 1-\frac12=\frac12\Longrightarrow a_6\ge2\\
a_7&=a_6-a_5\ge2-1=1\\
&\vdots
\end{aligned}
$$



By this process we could get $a_{4i}\le2,a_{4i+1}\le1,a_{4i+2}\ge2,a_{4i+3}\ge1$ for indices $\pmod{2n}$. With $2n=4k+2$, we would get for every even indices $2\le a_i\le2$, and for every odd indices $1\le a_i\le 1$. Hence it's the only solution in this case.

- It is similar when we have $a_{2i-1},a_{2i+1}\le 1$ . Notice that


$$
\prod_{i=1}^n (a_{2i-1}-1)(a_{2i+1}-1)=\prod_{i=1}^n(a_{2i-1}-1)^2\ge 0
$$



And $n$ is odd, so there's always $i$ that satisfies $(a_{2i-1}-1)(a_{2i+1}-1)\ge 0$.

- $n=2k$ ,  if there exists $a_{2i-1},a_{2i+1}\ge1$  for some positive integer $i$ , WLOG $a_1,a_3\ge1$ . Then we will have a process similar to the first case.

By this process we could get $a_{4i}\le2,a_{4i+1}\le1,a_{4i+2}\ge2,a_{4i+3}\ge1$ for indices $\pmod{2n}$. Hence $a_1=1$ and notice that $a_{2n}=a_1+a_{2n-1}\ge2$, only $a_{2n}=2,a_{2n-1}=1$ satisfies it. By knowing those three values it's sufficient to see that $a_{2i-1}=1,a_{2i}=2$ for $i=1,2,\dots,n$. 

- If there doesn't exist $(a_{2i-1}-1)(a_{2i+1}-1)\ge 0$ , we will have that


$$
\begin{aligned}
a_1\ge1\ge a_3\le1\le a_5\ge1\ge a_7\cdots\\
\Longrightarrow a_1=\frac1{a_{2n}}+\frac1{a_2}\ge a_3=\frac1{a_{2}}+\frac1{a_4}\\
\Longrightarrow \frac1{a_{2n}}\ge\frac1{a_4}\\
a_5\ge a_7\Longrightarrow \frac1{a_4}\ge\frac{1}{a_8}\\
\vdots\\
a_{4k-3}\ge a_{4k-1}\Longrightarrow \frac1{a_{4k-4}}\ge\frac1{a_{4k}}\\
\Longrightarrow a_4=a_8=\cdots=a_{2n}\\
\end{aligned}
$$



Since 


$$
\begin{aligned}
a_1=\frac{1}{a_{2n}}+\frac{1}{a_2}=\frac1{a_4}+\frac{1}{a_2}=a_3\\
a_1\ge1\ge a_3\Longrightarrow a_1=a_3=1\\
a_2=a_1+a_3=2,a_4=2\\
\end{aligned}
$$


Then it's sufficient to see that $a_{2i-1}=1,a_{2i}=2$ for all integer $1\le i\le n$ .
