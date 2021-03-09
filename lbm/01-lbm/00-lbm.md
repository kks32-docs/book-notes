# Lattice Boltzmann Method (LBM)
The Boltzmann equation represents the dynamics of the fluid on the macroscale and is described using the fundamental variable - its distribution function $f(x, \xi, t)$. The solution to the Boltzmann equation may often yield a solution to the Navier-Stokes equation. The force-free Boltzmann equation is a hyperbolic equation that describes the advection of distribution function $f$, with particular velocity $\xi$. The source term $\Omega (f)$ depends only on the local values of $f$ and not on its gradients. Traditional CFD requires an iterative approach to solve the nonlinear advection term $(u \cdot \nabla) u$, and this introduces numerical approximation errors. In contrast, discretized Boltzmann equation uses a very different approach that yields the exact advection term. 

## Overview
The basic quantity of the LBM is the *discrete velocity distribution functions* $f_i(x, t)$ often called particle populations. It represents desnity of particles with velocity $c_i = (c_{ix}, c_{iy}, c_{iz})$ at position $x$ and time $t$. The mass and momentum of $f_i$: $\rho (x, t) = \sum_i f_i(x, t) \quad \rho u(x, t) = \sum_i c_i f_i(x, t)$. The major difference between $f_i$ and continuous distribution function $f$ is that all of the argument variables of $f_i$ are discrete. $c_i$ are small discrete set of velocities. The point $x$ at which $f_i$ is defined are positioned on a square lattice in space with lattice spacing $\Delta x$. Additionally, $f_i$ is defined only at certain times $t$, separated by a time step $\Delta t$. The time step $\Delta t$ and lattice spacing $\Delta x$ represent time and space resolution. 

Velocity sets $\{c_i, w_i\}$, where $w_i$ is a weighting coefficient, are used to solve the Navier-Stokes equation. D1Q3, D2Q9, D3Q15, D3Q19, and D3Q27 are typical velocity sets. There is a trade-off between using smaller velocity sets for low memory consumption and higher accuracy of large velocity sets.

In isothermal LBE, the equation $p = c_s^2 \rho$ describes the relationship between the pressure $p$ and density $\rho$. Therefore, $c_s$ represents the isothermal model's speed of sound. In all velocity sets, $c_s^2 = (1/3) (\Delta x/\Delta t)^2$ is constant. By discretizing the Boltzmann equation in velocity and space, we get the Lattice Boltzmann equation:

$$f_i(x + c_i \Delta t, t+\Delta t) = f_i (x, t) + \Omega_i (x, t)$$

Particles $f_i(x, t)$ move with velocities $c_i$ to a neighboring point $x + c_i \Delta t$ at the next time step $t + \Delta t$. The collision operator $\Omega_i$ modes particle collision by redistributing particles among the population $f_i$ at each site. 

The Bhatnagar-Gross-Krook (BGK) collision operator:
$$\Omega_i (f) = - \frac{f_i - f_i^{eq}}{\tau} \Delta t$$
relaxes the population towards an equilibrium from $f_i^{eq}$ at a rate determined by the relaxation time. The equilibrium is given by:
$$f_i^{eq}(x, t) = w_i \rho \left(1 + \frac{u \cdot c_i}{c_s^2} +  \frac{(u \cdot c_i)^2}{2c_s^4} -  \frac{u \cdot u}{2c_s^2}\right)$$
$w_i$ weights specific to the velocity sets. The equilibrium is such that the moments are the same as $f_i$, i.e., $\sum_i f_i^{eq} = \sum_i f_i = \rho$ and $\sum_i c_i f_i^{eq} = \sum_i c_i f_i = \rho u$. The equilibrium $f_i^{eq}$ depends on the local quantities, density $\rho$ and fluid velocity $u (x, t) = \rho u (x, t) / \rho(x, t)$. 

Chapman-Enskog analysis links the Lattice Boltzmann Equation to the Navier Stokes Equation which results in macroscopic behavior, with the kinetic shear viscosity given by relaxation time $\tau$: $\nu = c_s^2 (\tau - \frac{\Delta t}{2})$, kinematic bulk viscosity $\nu_B = 2/3 \nu$, and viscous stress tensor $\sigma_{\alpha \beta} \approx - (1 - \frac{\Delta t}{2 \tau} \sum_i c_{i\alpha} c_{i \beta} f_i^{neq})$. The non-equilibrium $f_i^{neq} = f_i - f_i^{eq}$, is the deviation of $f_i$ from equilibrium. 

### LGBK
The LGBK equation is given as:
$$f_i(x + c_i \Delta t, t + \Delta t) = f_i (x, t) - \frac{\Delta t}{\tau} (f_i (x, t) - f_i^{eq} (x, t))$$
The LBE consists of two parts: *collision* and *streaming*. Collision is an algebraic local operation, which calculates the density $\rho$ and macroscopic velocity $u$ to find out the equilibrium distribution $f_i^{eq}$ and the post collision distribution $f_i^*$:

$$f_i^*(x, t) = f_i(x, t) (1-\frac{\Delta t}{\tau}) + f_i^{eq} (x, t) \frac{\Delta t}{\tau}$$

When $\delta t / \tau = 1$, we get $f^*_i = f_i^{eq}$. After collision, we *stream* the resulting distribution function $f^*$ to neighboring nodes. A single time step involves both these operations. 

$$f_i(x + c_i \Delta t, t + \Delta t)= f_i^*(x, t)$$

![LBM](lbm-collision-streaming.png)

### Algorithm
0. Initialize population as $f_i^{eq}(x, t = 0) = f_i^{eq}(\rho (x, t=0), u(x, t=0))$, density as $\rho(x, t= 0) = 1$ and velocity is zero $u(x, t =0) = 0$. 
1. Compute macroscopic moment $\rho(x, t)$ and $u(x, t)$ from $f_i(x, t)$. 
2. Obtain equilibrium distribution function $f_i^{eq}(x, t) = w_i \rho \left(1 + \frac{u \cdot c_i}{c_s^2} +  \frac{(u \cdot c_i)^2}{2c_s^4} -  \frac{u \cdot u}{2c_s^2}\right)$.
3. Perform *collision* (relaxation): $f_i^*(x, t) = f_i(x, t) (1-\frac{\Delta t}{\tau}) + f_i^{eq} (x, t) \frac{\Delta t}{\tau}$
4. Perform *streaming*: $f_i(x + c_i \Delta t, t + \Delta t)= f_i^*(x, t)$
5. Increase time step $t$ to $t + \Delta t$ and repeat from step 1. 

## Discretization in velocity space
Discretization in velocity space allows us to reduce the continuous 3D velocity space to a small number of discrete velocities without compromising the validity of macroscopic equations. 

In contrast to the unknown distribution function $f$ the equilibrium distribution function $f^{eq}$ is known function of exponential form. $f^{eq}$ can be represented through the exponential weight function. The mass and momentum can be represented as integral of $f^{eq}$ multiplied by Hermite polynomial. 

The Boltzmann equation is:  [#boltzmann-equation]
$$\left(\frac{\partial f}{\partial t}\right) + \xi_\alpha\left(\frac{\partial f}{\partial x_\alpha}\right) + \frac{F_\alpha}{\rho}\left(\frac{\partial f}{\partial \xi_\alpha}\right)=\Omega(f)$$

The Boltzmann equation describes the  evolution of the distribution function $f(x, \xi, t)$, i.e., density of particle with velocity $\xi$ at position $x$ and time $t$. In a force-free homogeneous steady state, the left-hand side disappears, and the solution of the Boltzmann equation is $f^{eq}$. $f^{eq}$ is written in terms of macroscopic quantities: density $\rho$, fluid velocity $u$, and temperature $T$. 

$$f^{eq}(\rho, u, T, \xi) = \frac{\rho}{(2\pi R T)^{d/2}} e^{-(\xi -u)^2/2RT}$$

where, $d$ is the number of spatial dimensions, gas constant $R = k_\beta / m$, $k_\beta$ is the Boltzmann constant and $m$ is the particle mass. 

Physical pheomena occurs at certain characteristic scales. Such as length $l$, velocity $V$, and density $\rho_0$. A characteristic time scale is $t_0 = l/V$. Using $*$ to denote non-dimensional quantities. 

$$\frac{\partial}{\partial t^*} = \frac{l}{v}\frac{\partial}{\partial t};  \quad \frac{\partial}{\partial x^*} = l\frac{\partial}{\partial x}; \quad \frac{\partial}{\partial \xi^*} = v\frac{\partial}{\partial \xi};$$

Non-dimensional Boltzmann equation:
$$\left(\frac{\partial f^*}{\partial t^*}\right) + \xi_\alpha^*\left(\frac{\partial f^*}{\partial x_\alpha^*}\right) + \frac{F_\alpha^*}{\rho^*}\left(\frac{\partial f^*}{\partial \xi_\alpha^*}\right)=\Omega^*(f^*)$$

$$f^* = f V d/\rho_0, \quad \quad F^* = F l / (\rho V^2)  \quad \quad \rho^* = \rho/\rho_0, \quad \quad \Omega^* = \Omega l V^2/\rho_0  \quad \quad \theta^* = RT/V^2$$

We will omit $^*$ to refer to Boltzmann equation in non-dimensional form. Force-free continuous Boltzmann equation is:
$$\left(\frac{\partial f}{\partial t}\right) + \xi_\alpha\left(\frac{\partial f}{\partial x_\alpha}\right) + =\Omega(f)$$

Conservation implies momentum of equilibrium distribution function $f^{eq}$ and particle distribution function $f$ coincide:

$$\int f(x, \xi, t)d^3\xi = \int f^{eq} (\rho, u, \theta, \xi) = \rho(x, t)$$
$$\int f(x, \xi, t) \xi d^3\xi = \int f^{eq} (\rho, u, \theta, \xi) \xi = \rho u(x, t)$$
$$\int f(x, \xi, t) \frac{|\xi|^2}{2}d^3\xi = \int f^{eq} (\rho, u, \theta, \xi)\frac{|\xi|^2}{2} = \rho E(x, t)$$
$$\int f(x, \xi, t)\frac{|\xi - u|^2}{2}d^3\xi = \int f^{eq} (\rho, u, \theta, \xi) \frac{|\xi-u|^2}{2}= \rho e(x, t)$$

The dependence on space and time in $f^{eq}$ enters only through $\rho(x, t)$, $u(x, t)$ and $\theta(x, t)$. 

**The idea of the Hermite series is to take the continuous integral to discrete sums evaluated at a particular velocity space (for specific values of $\xi$).**

## Hermite Polynomials (HP)
1D HP can be obtained from the *weight function* (also called the *generating function*):

$$\omega(x) = \frac{1}{\sqrt{2\pi}}e^{-x^2/2}$$

The weighting function allows us to construct 1D HP of n-th order ($n \ge 0$):

$$H^n(x) = (-1)^n \frac{1}{\omega(x)}\frac{d^n}{dx^n}\omega(x)$$

For different values of $n$:
$$H^{(0)}(x) = 1 \quad H^{(1)}(x) = x  \quad H^{(2)}(x) = x^2 -1  \quad H^{(3)}(x) = x^3 - 3x$$

For d-spatial dimensions:
$$H^n(x) = (-1)^n \frac{1}{\omega(x)}\nabla^{(n)}\omega(x) \quad \omega(x) = \frac{1}{(2\pi)^{d/2}}e^{-x^2/2}$$

$H^n$ and $\nabla^n$ has $d^n$ components: $\nabla^{(n)}_{\alpha_1, \dots \alpha_n} = \frac{\partial}{\partial x_{\alpha_1}}\dots\frac{\partial}{\partial x_{\alpha_n}}$

Derivatives are commutative: $\nabla_{xxy}^3 = \nabla_{xyx}^3 = \nabla_{yxx}^3$. We deal with $d = 2$ or $3$. $a_1, \dots a_n \in \{x, y\}$ or $\{x, y, z\}$. 

For $d=2$. HP up to second order ($n = 0, 1, 2$):
$$\nabla_{xx}^2 = \frac{\partial}{\partial x}\frac{\partial}{\partial x} \quad \nabla_{xy}^2 = \frac{\partial}{\partial x}\frac{\partial}{\partial y} \quad \nabla_{yx}^2 = \frac{\partial}{\partial y}\frac{\partial}{\partial x} \quad \nabla_{yy}^2 = \frac{\partial}{\partial y}\frac{\partial}{\partial y}$$

For $n = 0$:
$$H^{(0)} = 1$$
For $n = 1$:
$$H^{(1)}_x = - \frac{1}{e^{-(x^2 + y^2)/2}} \partial_x e^{-(x^2 + y^2)/2} = x$$
$$H^{(1)}_y = - \frac{1}{e^{-(x^2 + y^2)/2}} \partial_y e^{-(x^2 + y^2)/2} = x$$
For $n = 2$:
$$H^{(2)}_{xx} = - \frac{1}{e^{-(x^2 + y^2)/2}} \partial_x \partial_x e^{-(x^2 + y^2)/2} = x^2 -1$$
$$H^{(2)}_{xy} = H^{(2)}_{yx} = - \frac{1}{e^{-(x^2 + y^2)/2}} \partial_x \partial_y e^{-(x^2 + y^2)/2} = xy$$
$$H^{(2)}_{yy} = - \frac{1}{e^{-(x^2 + y^2)/2}} \partial_y \partial_y e^{-(x^2 + y^2)/2} = y^2 -1$$