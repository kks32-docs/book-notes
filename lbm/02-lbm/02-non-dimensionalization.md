# Non-dimensionalization
LBM simulations are performed in "lattice units," where all physical parameters are represented by non-dimensional numbers. Non-dimensional quantity $l^* = l/c_l$, where $l$ is the physical quantity, $c_l$ is the conversion factor. Length, time (or velocity), and density are natural quantities in LB simulations. Physics is independent of units, which is a human construct. The ratio of physical phenomena is what matters. Two incompressible flow systems are dynamically similar if they have the same Re and geometry ($Re = \rho l U / \eta$). Laws of similarity dictate, to have the same Re if the dimension of the geometry is five times smaller, the flow velocity or viscosity must be scaled by a factor of 5. For physical and non-dimensional Re to be equal:

$$\frac{l^*U^*}{v^*} = \frac{l U}{v}$$

Conversion factors ($c_l = l^* / l$), which yields $\frac{c_l c_u}{c_v} =1 \rightarrow c_v = c_l c_u$.

Kinematic viscosity ($m^2/s$) must have the conversion factor ($c_l * c_u$). $c_v \propto c_l c_u$, this is only a proportionality rather than equality. The law of similarity for Re, however, uniquely defines the relation of conversion factor for viscosity, length, and velocity. 

For a given basic conversion factors $c_l, c_t, c_\rho$ one can construct factors for pressure $P (kg/m^3) (m^2/s^2)$ as $c_p = c_\rho c_l^2 / c_t^2$.

- $\Delta x$ distance between neighboring lattice nodes in physical units (m)
- $\Delta t$ physical length of a time step (s)
- $\tau$ relaxation time (s)
- Dimensionless fluid density $\rho^*$ (the average fluid density is set to unity $\rho_0^* = 1$). Multiphase/multicomponent systems are slightly more complicated when density can be different from unity.
- Velocity $U^*$ typically non-dimensional but may need to specify $U^*$ as a prescribed velocity boundary.
- Lattice speed $c_s^* = 1/\sqrt{3}$. For stability $u^* << c_s^*$, typically this non-dimensional velocity value is less than 0.2.

Physical parameters $\Delta x, \Delta t, \tau, \rho, U$ are related to their lattice counterparts $\Delta x^*, \Delta t^*, \tau^*, \rho^*, U^*$. It is common to set $\Delta x^* = 1, \Delta t^* = 1, \rho^* =1$ (lattice units). This means conversion factors $c_l = \Delta x, c_t = \Delta t, c_\rho = \rho$. $\Delta x$ and $\Delta x^*$ **are same quantity in different unity system.**

The relaxation time is related to non-dimensional counterpart as $\tau = \tau^* c_t$. The conversion factor for velocity $c_u = c_l/c_t = \Delta x/\Delta t$ is not independent.

Kinematic viscosity ($c_v = c_l^2/c_t$) has units of  $(m^2/s)$. In LU, $v^* = (c_s^*)^2 (\tau^* - 0.5)$. The physical kinematic viscosity $v = (c_s^*)^2 (\tau^* - 0.5) \Delta x^2 / \Delta t$.

The pressure $p^* = c_s^* \rho^*$. However, only the pressure gradient $\nabla p$ appears in the NSE rather than the pressure $p$ itself. While $p$ appears in the energy equation, it is irrelevant for non-thermal LB. To connect physical pressure we decrease LB density to constant average density $\rho_0^*$ and derivative $\rho^{\prime *}$:

$$\rho^* = \rho_0^* + \rho^{\prime *}$$
It is wrongly assumed to connect reference density $\rho_0^*$ to physical reference pressure $p_0$ (atmospheric pressure).

$$p = p_0 + p^\prime = p_0 + p^\prime c_p^*, \quad p^{\prime *} = (c_s^*)^2 \rho^{\prime *}$$

$c_p = c_\rho c_l^2 /c_t^2 = c_\rho c_u^2$. $p_0$ **reference pressure can be freely specified**. Stress has the same conversion factor as pressure. The force conversion factor $c_f = c_\rho c_l^4/c_t^2$. Body force (force/volume), $c_F = c_f / c_l^3 = c_\rho c_l/c_t^2$. Gravitational force $F_g = \rho g$ conversion factor $c_l / c_t^2$.

## Error terms in LB
- Spatial discretization error scales $\Delta x^2$
- Time discretization error scales $\Delta t^2$
- Compressibility error in incompressibility limit is $Ma^2 \propto U^{*2}$. $U^*$ decreases with increasing $C_u$. $C_u = c_l / c_t = \Delta x / \Delta t$ error scales $(\Delta t / \Delta x)^2$
- BGK truncation error is $\propto (\tau - 1/2)^2$

Changing $\Delta x$ alone does not reduce error because compressibility error $(\Delta t^2/ \Delta x^2)$ will eventually be the controlling factor.

Diffusive scaling $(\Delta t = \Delta x^2)$ of $\tau^*$ means as the resolution of grid $\Delta x$ is refined, the overall velocity error then decreases proportional to $\Delta x^2$.

Acoustic scaling $(\Delta t \propto \Delta x)$ keeps compressibility error unchanged. To converge incompressible NSE, $\Delta t = \Delta x^\gamma$, with $\gamma > 1$, the compressibility error remains constant at $\gamma = 1$. 

## Stability

$\tau^*$ should not be too close to 1/2. Velocity $U^*$ should be larger than about 0.4, for $\tau^* > 0.5$. Maximum velocity decreases when $\tau^*$ approaches $1/2$. For $\tau^* < 0.55$:

$$\tau^* > \frac{1}{2} + \alpha U^*_{max}$$

$\alpha$ is a numerical constant in the order of 1/8. If $\tau^* = 0.51$, the maximum velocity should be below 0.08. If the expected maximum velocity is 0.01, then $\tau^*$ should be as low as 0.50125. 

Lattices should be sufficiently fine to resolve local vortices. Simulation is stable as long as the relevant length scales are resolved. 

## Efficiency

Runtime: $T \propto \frac{1}{\Delta x^d \Delta t}$

Memory: $M \propto \frac{1}{\Delta x^d}$

## Parameter selection
- If spatial or temporal resolution shall be refined or coarsened, one must obey $\Delta t \propto \Delta x^2$
- $\tau^*$ should be close to unity if possible.
- $\tau^*$ close to 1/2, $\tau^* > \frac{1}{2} + \alpha U^*_{max}$
- $U^*_{max}$ should not exceed 0.4 to maintain stability (typically between 0.03 - 0.1).

Reynold's number typically influences the flow, unless for vanishing small Re (like Stokoe's flow). In LB simulations of incompressible flow, $Ma$ are small and are not required to map the exact values. 

Based on laws of similarity:

$$\nu = c_s^{*2} (\tau^* -0.5) \frac{\Delta x^2}{\Delta t}$$

This means that the three simulation parameters $\Delta x$, $\Delta t$, and $\nu$ are not independent, and only two of them can be chosen freely. 

If we now additionally claim that the lattice and physical $Ma$ shall be identical:

$$ Ma^{(l)} = Ma^{(p)} = \frac{U^*}{c_s^*} =  \frac{U}{c_s}$$
$$\frac{\Delta x}{\Delta t} = c_u = \frac{U}{U^*} = \frac{c_s}{c_s^*}$$

Since $c_s^*$ is known, $c_s$ is defined by the physical sound speed ratio of $\Delta x / \Delta t$ is fixed by the Mach number (acoustic scaling). In situations where compressibility effects are not desired, Ma can be dropped. It is computationally more efficient to increase the lattice Ma, since timestep scales $\frac{1}{U^*} \propto \frac{1}{Ma}$.

Relation of simulation parameters in Re and Ma:

$$ \frac{Re^{(l)}}{Ma^{(l)}} = \frac{l}{c_s^* (\tau^* - 0.5) \Delta x}$$

$$l^* = l / \Delta x$$. $Ma^{(l)}/Re^{(l)}$ is the lattice Knudsen number. For $\tau^* = O(1)$, Kn is basically $\Delta x / l$. Hydrodynamic behavior is only expected for small Kn, which sets an upper bound for lattice constant $\Delta x$:

$$\tau^* = \frac{1}{2} + \frac{U^*_{max}}{c_s^{*2} Re_g$$

$Re_g$ is the grid Reynolds number.

### Small Reynolds number
Small Reynolds number can be reached by choosing a large $\Delta x$ or a large $\tau^*$ or a small lattice Ma. It is not advisable to use $\tau^* >> 1$. To reduce Re we must decrease Ma, therefore the flow velocity $U^*$, this means $\Delta t = \Delta x / c_u$, where $c_u \propto 1/Ma$ and $\Delta t \propto Ma$. 

$$Re^{(l)} = U^* \frac{100}{3/2 * c_s^{*2}$$

For $Re = 0.01$, max $U^* \approxeq 1e-4$ which requires a large number of time steps. At this point, it is advisable to consider a violation of Re (capillary flow), and time scales are not given by viscosity. 