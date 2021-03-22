# Chapman-Eskog Analysis

The assumption $f \approx f^{eq}$ result in Euler momentum equation. Any macroscopic behavior beyond the Euler equation must be connected to the non-
equilibrium part of $f$, i.e., $f^{neq} = f - f^{eq}$. Chapman-Eskog analysis:

$$f_i = f_i^{eq} + \epsilon f_i^{(1)}+ \epsilon^2 f_i^{(2)} + \dots$$

$f_i^{eq}$ is often written as $f_i^{(0)}$. Each order in Knudsen number ($Kn$) forms a semi-independent equation by itself. Consequently, the higher-order terms can be seen as correction terms, similar to viscous-stress tensor in NSE is a correction term to Euler equation.

In petrubation analysis, only the lowest two orders are required to find the NSE, we do not need to look closely at higher-order components of $f_i$ than $f_i^{eq}$ and $f_i^{(1)}$:

$$f_i(x + c_i \Delta t, t + \Delta t) - f_i (x, t) = -\frac{\Delta t}{\tau} (f_i(x, t) - f^{eq}(x, t))$$

Like any collision operator, BGK must conserve mass and momentum.

$$\sum_i f_i^{neq} = 0; \quad \sum_i c_i f_i^{neq} = 0$$

This assumption holds individually at each order:

$$\sum_i f_i^{n} = 0; \quad \sum_i c_i f_i^{n} = 0 \quad n \ge 1$$

## Taylor expansion of LBE

$$\Delta t (\partial_t + c_{i\alpha} \partial_\alpha) f_i + \frac{\Delta t^2}{2} (\partial_t + c_{i\alpha} \partial_\alpha)^2 f_i + O(\Delta t^3) = -frac{\Delta t}{\tau}f_i^{neq}$$

The equation is continuous in time and space but retains the discretization error of LBE. We neglect third and higher-order terms, as changes in $f_i$ are slow occurring only on a macroscopic scale. We can get rid of second-order derivative terms by subtracting $\frac{\Delta t}{2}(\partial_t + c_{i\alpha} \partial_\alpha)$ applied to the equation itself:

$$\Delta t (\partial_t + c_{i\alpha} \partial_\alpha) f_i = -frac{\Delta t}{\tau}f_i^{neq} + \Delta t (\partial_t + c_{i\alpha} \partial_\alpha) \frac{\Delta t}{2 \tau} f_i^{neq} $$

It is also necessary to expand the time derivatives in terms spanning several orders in Kn. The time and space derivatives become:

$$\Delta t \partial_t f_i = \Delta t (\epsilon \partial_t^{(1)} f_i + \epsilon^2 \partial_t^{(2)}f_i + \dots) \quad \quad \Delta t c_{i\alpha}\partial_\alpha f_i = \Delta t (\epsilon c_{i\alpha}\partial_\alpha^{1})f_i$$

Apply the expansion of $f_i$ and time-derivative expansion:

$$O(\epsilon): \quad (\partial t^{(1)} + c_{i\alpha}\partial_\alpha^{(1)}) f_i^{eq} = -\frac{1}{\tau} f_i^{(1)})$$
$$O(\epsilon^2): \quad \partial_t^{(2)}f_i^{eq} + (\partial t^{(1)} + c_{i\alpha}\partial_\alpha^{(1)})(1- \frac{\Delta t}{2\tau})f_i^{(1)} = -\frac{1}{\tau}f_i^{(2)}$$

The continuity equation is exact already at $O(\epsilon)$. However, $O(\epsilon^2)$ correction is non-zero. $O(u^3)$ error term can be neglected if $u^2 << c_s^2$, which is equivalent to the condition of $Ma^2 << 1$ ($Ma = u /c_s$). Hence, LBE is only applicable for the *weakly compressible* form as opposed to *strongly compressible*, which occurs for transonic and supersonic flows, where $Ma$ goes towards unity and beyond. 

$$p = \rho c_s^2; \quad\quad \eta = \rho c_s^2 (\tau - \frac{\Delta t}{2}); \quad \quad \eta_\beta = \frac{2}{3}\eta$$

We obtain the bulk viscosity $\eta_\beta$ to be a function of shear viscosity $\eta$, base on the monatomic kinetic theory. However, the bulk viscosity is normally found to be zero. The difference is caused by the use of isothermal EOS, which is incompatible with monatomic assumptions. 

For viscosity equation, $\tau/\Delta t \ge 1/2$ is a necessary condition for stability, as $\tau / \Delta t < 1/2$ would lead to macroscopically unstable situation with negative viscosity. 

The Hermite polynomial approach yields velocity sets $c_i$ and equilibrium functions $f_i^{eq}$, so that zero to second-order  moments $\Pi^{eq}$, $\Pi_\alpha^{eq}$, $\Pi_{\alpha\beta}^{eq}$ of $f_i$ equal those of the continuous velocity distribution $f^{eq}$. However, the third-order moment $\Pi_{\alpha\beta\gamma}^{eq}$ is not fully correct leading to $\rho u_{\alpha}u_\beta u_\gamma$ error term in macroscopic momentum equation. 

In all velocity sets, we find $c_{i\alpha} \in \{-1, 0, +1\} \frac{\Delta x}{\Delta t}$, so that $c_{i\alpha}^3 = c_{i\alpha}(\frac{\Delta x}{\Delta t})^2$. Consequently,
$$\Pi^{eq}_{xxx} = \sum_i c_{ix}^3 f_i^{eq} = \left(\frac{\Delta x}{\Delta t}\right)^2 \sum_i c_{ix} f_i^{eq} = \left(\frac{\Delta x}{\Delta t}\right)^2 \Pi^{eq}_x.$$
The third order moments are proportional to the first order $x, y, z$ moments respectively. Other higher-order moments are equal to lower order although specific equality vary between velocity sets. For D2Q9 velocity set, we can find that $c_{i\alpha}^3 = c_{i\alpha}\left(\frac{\Delta x}{\Delta t}\right)^2$ that there are only nine independent moments. As some third-order moments are related to first order moments it is not possible for the velocity set to correctly recover $\rho u_\alpha u_\beta u_\gamma$ term in $\Pi_{\alpha\beta\gamma}^{eq}$ and thus avoid the $O(u^3)$ error term which carries then to the macroscopic stress tensor. 

## Time-scale interpretation
If we assume the decomposition into different time scales as the clock ticking at a different speed, this may lead to false conclusions. In steady-state when the time derivative $\partial_t g$ of a function $g$ vanishes all "time scale" derivatives $\partial_t^{n}g$ can be removed as well. After all, "steady-state" means that nothing can change at any time scale. 

Macroscopically, Poiseuille flow obeys $\Delta p = \Delta \cdot \sigma$, i.e., flow is driven by pressure gradient which is exactly balanced by the divergence of viscous stress tensor $\sigma$. The moments of $\epsilon$ and $\epsilon^2$-scale:

$$\partial_t^{(1)}(\rho u_\alpha)= -\partial_\beta^{(1)} \Pi_{\alpha\beta}^{eq} \quad \quad \partial_t^{(2)}(\rho u_\alpha)= -\partial_\beta^{(1)} \left( 1 - \frac{\Delta t}{2\tau} \right) \Pi_{\alpha\beta}^{eq}.$$

Obviously, $\partial_t (\rho u_a) = 0$ holds for steady Poiseuille flow. From time-scale terms $\partial_t^{(1)} (\rho u_a)$ and $\partial_t^{(2)} (\rho u_a)$ must also vanish independently. However, this leads to $\partial_\alpha^{(1)} p = 0$ and $\partial_\beta^{(1)} \sigma_{\alpha\beta}=0$, i.e., no flow at all! Poiseuille flow would be impossible if $\partial_t^{(1)} (\rho u_a)$ and $\partial_t^{(2)} (\rho u_a)$ vanishes indepdentl. Hence it is incorrect to say time discretized into different time scales. The term $\partial_t g$ is decomposed into contributions from different orders in petrubation expansion, contribution which thmeselvers not time derivatives. Only the sum of these contributions vanishes.