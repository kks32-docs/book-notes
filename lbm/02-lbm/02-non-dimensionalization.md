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