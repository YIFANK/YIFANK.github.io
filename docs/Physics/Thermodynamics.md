### The Ideal Gas Law

$$
pV=nRT=Nk_BT,p=\dfrac{\rho k_BT}{m},U=\alpha nRT
$$

where:

- $p$ is the absolute pressure of the gas,
- $V$ is the volume of the gas,
- $n$ is the number of moles,
- $R$ is the ideal gas constant,
- $k_B$ is the Boltzmann constant,
- $N_A$ is the Avogadro constant,
- $T$ is the absolute temperature of the gas,
- $\alpha$ is the number of degrees of freedom divided by $2$. ($\alpha = 3/2$ for monatomic gas, $\alpha = 5/2$ for diatomic gas).
- $N$ is the number of particles.

### Van der Waals equation


$$
(p+\dfrac{a}{V^2})(V-b)=RT,p=\dfrac{RT}{V-b}-\dfrac{a}{V^2}
$$


### The First Law of Thermodynamics

$$
dU= dW +dQ,dW=-pdV
$$

### The Second Law of Thermodynamics 

$$
dS=\dfrac{dQ}{T}
$$

### Adiabatic Process


$$pV^\gamma = \text{constant}$$

$$p^{1-\gamma}T^\gamma = \text{constant}$$

$$TV^{\gamma - 1}=\text{constant}$$


where $\gamma$ is the adiabatic index or heat capacity ratio defined as:


$$
\gamma=\dfrac{C_p}{C_V} = \dfrac{f+2}{f}
$$

- $C_p,C_V$ is the specific heat for constant pressure, $C_V$ is the specific heat for constant volume, and $f$ is the number of *degrees of freedom* (3 for monoatomic gas, 5 for diatomic gas or a gas of linear molecules). 

**Derivation for P-V relation:**

The adiabatic process is defined as a process where heat transfer is zero, then we can apply the first law of thermodynamics:


$$
dQ=dU-dW=d(\alpha pV)+pdV=(\alpha +1)pdV+\alpha Vdp=0
$$

We can separate the variable:


$$
-(\alpha  + 1)\dfrac{dV}V=\alpha\dfrac{dp}p
$$

Integrate on both sides:


$$
\ln(p/p_0)=-\dfrac{\alpha + 1}{\alpha}(V/V_0)
$$

And then exponentiate them, substituting $\gamma=\dfrac{\alpha + 1}{\alpha}$ .

$$
(p/p_0)=(V/V_0)^{\gamma}\Longrightarrow pV^\gamma = \text{const}
$$
Similar derivations can be done for Van der Waals equation.
