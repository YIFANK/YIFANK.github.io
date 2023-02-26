Evaluate the indefinite integral


$$
\int\bigg(\sqrt{x+1}-\sqrt{x}\bigg)^{\pi}dx
$$


First, we can substitute the thing inside the bracket with $u$. Let $u= \sqrt{x+1}-\sqrt{x}$, then


$$
\frac{du}{dx} =\frac12\bigg(\frac1{\sqrt{x+1}}-\frac1{\sqrt x}\bigg)=-\frac{u}{2\sqrt{x(x+1)}}
$$


Also, we notice that


$$
u^2 = 1 - 2\sqrt{x(x+1)},u^{-2}= 1+2\sqrt{x(x+1)}
$$


This implies


$$
\sqrt{x(x+1)}=\frac{u^{-2}-u^2}{4}\implies \frac{du}{dx}=\frac{2u}{u^2-u^{-2}}\\
dx=\frac{1}{2}(u+u^{-3})du
$$


Thus,


$$
\begin{aligned}
\int\bigg(\sqrt{x+1}-\sqrt{x}\bigg)^{\pi}dx &=\frac12\int(u^{\pi+1}-u^{\pi-3})du\\
&=\frac12\bigg(\frac{u^{\pi+2}}{\pi+2}-\frac{u^{\pi-2}}{\pi - 2}\bigg)+C\\
&=\frac12\bigg(\frac{(\sqrt{x+1}-\sqrt x)^{\pi+2}}{\pi + 2}-\frac{(\sqrt{x+1}-\sqrt x)^{\pi-2}}{\pi - 2}\bigg)+C\\
\end{aligned}
$$






