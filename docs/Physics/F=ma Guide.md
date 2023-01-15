The syllabus is inspired by the video [F=ma Complete Guide](https://www.youtube.com/watch?v=SuFpSrCzucM). First, there are some helpful things to know about before taking the F=ma exam:

- 75 minutes, 25 questions
- Calculators are allowed
- 5 choices per question
- 1 point per correct question, no penalty for guessing.
- Score 14-18 to qualify for USAPhO
- 400 out of 6000 qualify
- Average score = 8/25

### Dimensional Analysis

**Dimensional Analysis** is very useful in both physics and engineering. It is the analysis of the relationships between different **physical quantities** (e.g acceleration, pressure, velocity, force) by identifying their **base quantities** (e.g. length, mass, time) and their **units** (meters, kilograms, seconds). It is important to remember how units are related through formulas!

For example, in kinematics, we use the equations of motion to describe the motion of an object. One of the equations is:


$$
x = x_0 + v_0t + (1/2)at^2
$$


Where $x$ is the final position of the object, $x_0$ is the initial position, $v_0$ is the initial velocity, $t$ is the time, and $a$ is the acceleration.

We can use dimensional analysis to check that the units of the different terms in the equation are consistent. The units of $x$, $x_0$, and $v_0$ are length (e.g. meters), the units of $t$ are time (e.g. seconds), and the units of $a$ are length per time squared (e.g. meters per second squared).

So we can check that the equation is dimensionally consistent by making sure that the units on both sides of the equation are the same.
$$
[L] = [L] + [L/T]\times[T] + (1/2)[L/T^2]\times[T^2]
$$
On both sides of the equation, we have length units, which means that the equation is dimensionally consistent and it's a correct equation of motion.

### Error Propagation

In statistics, propagation of uncertainty (or propagation of error) is the effect of variables' uncertainties on the uncertainty of a function based on them.

The uncertainty u can be expressed in a number of ways. e.g. the absolute error $\delta x$ , the relative error $\delta x/x$.

If $x$ has uncertainty (error) $\delta x$ , then $cx$ has uncertainty $c\delta x$ .

If $x$ and $y$ have independent random errors $\delta x$ and $\delta y$ , then the error in $z=x+y$ is


$$
\delta z=\sqrt{\delta x^2+\delta y^2}
$$

the error in $z=x\times y$ or $z=x/y$ is:


$$
\frac{\delta z}{z}=\sqrt{(\frac{\delta x}{x})^2+(\frac{\delta y}{y})^2}
$$

If $z=f(x)$ for some function $f$, then


$$
\delta z=|f'(x)|\delta x
$$

Thus, $x^n$ has relative uncertainty:


$$
\delta(x^n)/x^n=|n|\delta x/x
$$

Practice problems: 2018AP12,2018AP25,2019AP11,2019AP16,2019BP18,2021P8,2020AP23,2020BP25

Challenge problem:2019BP25

### Kinematics

All formulas in kinematics is essentially just:


$$
v=\dfrac{dx}{dt},a=\dfrac{dv}{dt}=\dfrac{d^2x}{dt^2}
$$


If you have some knowledge of calculus, then you don't even need to memorize any other formulas! But I will still put some useful formulas in constant acceleration motion below:

- The relationship between initial and final velocity: $v_f=v_0+at,v_f^2-v_0^2=2ax$
- finding distance: $x=v_0t+\frac 12at^2,x=(v_0+v_f)t/2$

### Projectile Motion 

In projectile motion, the horizontal motion and the vertical motion are **independent** of each other; that is, neither motion affects the other. When we analyze these problems, we try to separate the $x$ and $y$ components, set the equations up, solve them separately and combine our answers.

Here are some useful formulas to memorize, but make sure you do know how to derive them! However, what usually matters more is how well you understand them instead of memorization.

**Time of flight**

$$
t=\dfrac{2v_0\sin\theta}g
$$


**Maximum height reached**


$$
y_{max}=\dfrac{v_0^2\sin^2\theta}{2g}
$$


**Horizontal range**


$$
R=\dfrac{v_0^2\sin(2\theta)}{g}
$$


**Derivation**

Let the projectile be launched with an initial velocity $v_0$ at angle $\theta$ with respect to the horizontal surface, then its $x$ and $y$ components will be:



$$
v_{0x}=v_0\cos\theta,v_{0y}=v_0\sin\theta
$$



The object reaches the highest point when its $v_y$ gets to zero. Thus:


$$
t=v_{0y}/g,y_{max}=v_{0y}t-\frac12gt^2=\frac{v_{0y}^2}{2g}=\dfrac{v_0^2\sin^2\theta}{2g}
$$


When reaching the farthest point in $x$ direction, the object falls on the ground, by symmetry we know that this process takes twice the time of reaching the highest point, so:


$$
R=v_{0x}\times 2t=\frac{2v_{0x}v_{0y}}{g}=\frac{v_0^2\times 2\cos\theta\sin\theta}{g}=\frac{v_0^2\sin(2\theta)}{g}
$$


The last step is done with double angle formula $2\cos\theta\sin\theta =\sin(2\theta)$ .

When we are throwing an object up to a ramp with angle $\phi$, things will be more complicated. In this case, we can think of the projectile motion as a quadratic function and solve for the intersection. An alternative solution would be shifting the axis of our plane so that the $x$ axis is the ramp and $y$ axis perpendicular to it. 

### Forces and Newton's Laws

Common forces that you need to consider in your free body diagram:

- Gravitational force $mg$
- Spring force $-kx$
- Centripetal force $mv^2/r$
- normal force $N$
- friction force $f\le \mu N$ opposes the motion of the body

Some common models that you want to be familiar with:

- inclined plane
- pulleys
- rope
- rod

### Work and Energy

- Work: $W=F\cdot s=Fs\cos\theta$
- Kinetic energy: $E_k=\frac12mv^2$
- Gravitational potential energy: $E_p = mgh$
- Spring potential energy: $U=\frac12kx^2$
- Potential energy-force relationship: $F=-\dfrac{dU}{dx}$

- Work-energy theorem: $W=\Delta K$

### Rotating Objects

- torque $\tau$
- angular momentum $L$
- moment of inertia $I$
- rotational kinetic energy $K_r=\dfrac12I\omega^2$

### Gravitation


$$
F=G\frac{Mm}{r^2},U(r)=-\int_\infty^r Fdr=-\frac{GMm}{r}
$$


- **Kepler's First Law**: a planet moves around a star in an elliptical orbit
- **Kepler's Second Law**: a line connecting the planet and the star sweeps out equal area in equal times.
- **Kepler's Third Law**: square of the orbital period is proportional to cube of the semimajor axis. $T^2\propto a^3$

*Remark:* The **second law** is a result of conservation of **angular momentum**, while the other two laws requires the gravitational force to be exactly inverse proportional to square of the distance between two stars. ($F\propto r^{-2}$)

### Fluids

- **Archimedes principle** states that the upward buoyant force that is exerted on a body in a fluid is equal to the weight of the fluid that the body displaces. $F_b=\rho_lVg$, where $\rho_l$ is the density of the fluid and $V$ is the volume of the fluid that the body displaces.
- **Continuity equation**: basically it means that the volume flow in equals the volume flow out in any period of time. $\rho_1A_1v_1=\rho_2A_2v_2$. Most of the time we are dealing with incompressible liquids, so we can drop the density factor and get $A_1v_1=A_2v_2$.
- **Bernoulli equation**: the energy version of continuity equation  $P_1+\dfrac12\rho v_1^2+\rho gh_1=P_2+\dfrac12\rho v_2^2+\rho gh_2$

### Oscillations

**Simple harmonic motion**


$$
x(t)=A\cos(\omega t+\phi)
$$


Where $t$ is the time, $x$ is the displacement, $\omega$ is the angular frequency, $A$ is the amplitude, and $\phi$ is the phase shift measured in radians. This is a solution to the differential equation in either of the following forms:


$$
\begin{aligned}
a=\frac{d^2x}{dt^2}=-\omega^2x\\
x^2+\frac{v^2}{\omega^2}=A^2\\
\end{aligned}
$$


The first equation is derived from Newton's second law while the second one is a result of energy conservation. They are both very important to know about. Take the most classical example of a mass $m$ that moves horizontally while attaching to a spring with spring constant $k$. If the system is left at rest at the equilibrium position, then no net force is acting on the mass. Otherwise, we would have the restoring elastic force by the Hooke's law:


$$
F=-kx
$$


Notice that from Newton's second law we also have $F=ma$, then we can get:


$$
a=-\frac km x=-\omega^2x,\omega = \sqrt{\frac km}
$$


An alternative way is using the conservation of energy:


$$
\frac12mv^2+\frac12kx^2=\frac12mv_0^2+\frac12kx_0^2=\text{const}
$$


We can also get the angular frequency by dividing the factors of $x^2$ by the factors of $v^2$ in this formula. Below are some more formulas for some terms we use in oscillation:

- period $T$ of an oscillation: $T=2\pi/\omega$
- frequency $f$: $f=\omega/2\pi$



A system of $N$ coupled oscillators has $N$ different eigenmodes when all the oscillatros oscillate with the same frequency $\omega_i,x_j=x_{j0}\sin(\omega_i t+\varphi_{ij})$ and $N$ eigen-frequencies $\omega_i$ (which can be multiple, $\omega_i=\omega_j$). General solution is a superposition of all the eigenmotions.

### Waves (?)
