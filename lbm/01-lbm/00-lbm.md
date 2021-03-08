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