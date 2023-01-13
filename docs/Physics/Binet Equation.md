### Binet Equation

The shape of an orbit is often described in terms of relative distance $r$ as a function of $\theta$ . By Binet Equation, we can solve for the force in terms of the function $u(\theta)$ , where $u = \frac{1}{r}, h = L/m$ is the special angular momentum. 


$$
F=-mh^2u^2(\frac{d^2u}{d\theta^2}+u)
$$

#### Derivation

Newton's Second law for purely central force is


$$
F(r)=m(\ddot{r}-r\dot{\theta}^2)
$$

The conservation of momentum requires that

$$
r^2\dot{\theta}=h=\text{constant}
$$

Rewrite the derivative of $r$ with respect to time as derivatives of $u=1/r$ with respect to angle:


$$\frac{du}{d\theta}=\frac{d}{dt}\bigg(\frac1r\bigg)\frac{dt}{d\theta}=\frac{-\dot{r}}{r^2\dot{\theta}}=-\frac{\dot{r}}{h}$$

$$
\frac{d^2u}{d\theta^2}=\frac{d}{d\theta}\bigg(-\frac{\dot{r}}{h}\bigg)=-\frac{1}h\frac{d\dot{r}}{dt}\frac{dt}{d\theta}=-\frac{\ddot{r}}{h\dot{\theta}}=-\frac{\ddot{r}}{h^2u^2}
$$

Thus,


$$
\dot{r}=-h\frac{du}{d\theta}
$$

$$
\ddot{r}=-h^2u^2\frac{d^2u}{d\theta^2}
$$

Replace it back into the original equation for our central force, we get the Binet Equation as desired.
