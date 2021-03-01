# Basics of Hydrodynamics
## Continuity Equation
Consider a small fluid element with density $\rho$, which occupies some stationary volume $v_0$. The mass of the element is $\int_{v_0} \rho dv$. If we consider the change of this mass per unit time, it must be due to fluid flowing in or out of the volume to conserve the mass of the system:

$$\frac{\partial}{\partial t} \int_{v_0} \rho dv = \oint_{\partial v_0} \rho u dA$$
where the close area integral is taken over the boundary $\partial v_0$, $u$ is the fluid velocity and outward normal as the direction of $dA$. Converting surface to volume integral using divergence theorem:

$$\int_{v_0} \frac{\partial \rho}{\partial t} = - \int_{v_0} \nabla \cdot (\rho u) dv$$

Since $v_0$ is stationary and arbitrary. The **continuity equation** reflects the conservation of mass as:

$$\frac{\partial \rho}{\partial t} + \nabla \cdot (\rho u) = 0$$

The vector $\rho u$ is the *momentum density* or *mass flux density*. 

$$\frac{\partial \rho}{\partial t} + u \cdot \nabla \rho + \rho \cdot \nabla u = 0 \quad or \quad \frac{D \rho}{Dt} + \rho \cdot \nabla u = 0$$

The material derivative $\frac{D}{Dt} = \frac{\partial}{\partial t} + u \cdot \nabla$ denotes the rate of change as the fluid element moves about in space, rather than the rate of change of $\frac{\partial}{\partial t}$ at a fixed point in space. 

## Navier-Stokes Equation (NSE)
For a simple ideal fluid, the change of net momentum can be due to: (i) flow of momentum into or out of element, (ii) difference in pressure $p$, and (iii) external body force. 

$$\frac{d}{dt}\int_{v_0} \rho u dt = \oint_{\partial v_0} (\rho u u) \cdot dA - \oint_{\partial v_0} p dA + \int_{v_0} F dv$$

Transforming surface to volume integrals using the divergence theorem:

$$\int_{v_0} \frac{d \rho u}{dt} dt = \int_{v_0} \nabla \cdot (\rho u u) dA - \int_{v_0} \nabla p dA + \int_{v_0} F dv$$

**Euler's equation** describes the *conservation of momentum*:

$$\frac{\partial (\rho u)}{\partial t} + \nabla \cdot (\rho u u) = - \nabla p + F$$

The Cauchy momentum equation is a general form of conservation of momentum.

$$\frac{\partial (\rho u)}{\partial t} + \nabla \cdot \Pi = F$$

The momentum flux density $\Pi_{\alpha \beta} = \rho u_\alpha u_\beta - \sigma_{\alpha\beta}$, where $\sigma_{\alpha\beta}$ is the stress tensor. For simple fluid in an isotropic stress $\sigma_{\alpha\beta} = - p \delta_{\alpha\beta}$. 

The momentum flux transfer in Euler's equation only refers to reversible momentum transfer either due to the flow of mass or pressure changes. Real fluids need a viscosity or inertial friction term, which is dissipative. 

The viscosity stress tensor in separated into a traceless shear stress and normal stress components. The coefficients $\eta$ and $\eta_B$ correspond to the shear viscosity and bulk viscosity, respectively. The total stress tensor is the sum of pressure and viscosity terms. $\sigma_{\alpha\beta} = \sigma_{\alpha\beta}^\prime - p \delta_{\alpha\beta}$. Using this stress tensor in Cauchy momentum equation, we derive the **Navier-Stokes Equation (NSE)**. If the flow is regarded as incompressible ($\rho=const$), the continuity equation reduces to $\nabla \cdot u = 0$, then the incompressible NSE:

$$\rho \frac{Du}{Dt} = -\nabla p + \eta \Delta u + F$$

$\Delta = \nabla \cdot \nabla = \frac{\partial^2}{\partial x_\alpha \partial x_\beta}$ is the Laplace operator. 

## Equation of State (EOS)
We have five unknowns (density $\rho$, pressure $p$, three components of velocities), but only four equations (continuity equation, which is the conservation of mass, and three components of momentum from either Navier-Stokes equation or Euler's equation). We can solve this by either fixing a variable, such as assuming incompressibility condition $\rho = const$) Alternatively, use an additional equation. 

*State principle of equilibrium thermodynamics* relates the state variables that describe the local thermodynamics state of the fluid, such as density $\rho$, the pressure $p$, temperature $T$, internal energy $e$, and entropy $s$. State principle declares any state variable can be related to any other two state variables through an equation of state (EOS). 

The most famous EOS is the *ideal gas law*: $p = \rho R T$, which relates the pressure to density and temperature through a specific gas constant $R$. Introducing an EOS adds a new variable, in this case, temperature $T$, and does not make our system solvable. Instead, introducing EOS allows us to make suitable approximations such as assuming fluid has a constant temperature ($T \approx T_0$), which is the isothermal condition that yields $p = \rho R T_0$. 

Another EOS for an ideal gas is the expression of pressure as a function of density and entropy. 

$$\frac{p}{p_0} = \left(\frac{\rho}{\rho_0}\right)^\gamma e^{(s-s_0)/c_v}$$

$c_v$ and $c_p$ are heat capacities at constant volume and pressure, respectively and $gamma = \frac{c_v}{c_p}$ is the *adiabatic index*. For small derivatives, this equation can be linearized: $p = p_0 +p^\prime \approx p_o + \left(\frac{\partial p}{\partial \rho}\right)_p p^\prime + \left(\frac{\partial p}{\partial \rho}\right)_s s^\prime$.

For isentropic EOS, $p \approx p_0 + c_s^2 \rho^\prime$, since speed of sound $c_s$ is in general given by $c_s^2 = \left(\frac{\partial p}{\partial \rho}\right)_s$. For isentropic and isothermal EOS, we find $c_s = \sqrt{\gamma R T}$ and $c_s = \sqrt{RT_0}$, respectively. The constant reference pressure $p_0$ is insignificant to NSE as only pressure gradient $\nabla p = \nabla (p_0 + p^\prime) = \nabla p^\prime$ is present. Therefore the isothermal EOS is $p = c_s^2 \rho \rightarrow c_s^2 \rho_0 + c_s^2 \rho^\prime$ can be used to model EOS in the linear regime where entopy is nearly constant, as long as speed of sound is matched, it does not matter if the reference pressure $p_0$ is different. 