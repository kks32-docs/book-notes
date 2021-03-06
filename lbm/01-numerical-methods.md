# Numerical Methods
## Finite Difference Method
The finite Difference Method (FDM) takes a set of equations and replaces the derivatives with finite difference approximations. The central difference scheme is more accurate than the forward or backward difference. However, in the advection term $\partial (\rho_\alpha u_\beta)/\partial x_\beta$, information comes only from the direction opposite of the flow, i.e., *upstream or upwind*. Since the central difference looks at both the upstream and downstream, it is possible to improve it by using an *upwind scheme*, where either a forward or backward scheme is used depending on the direction of fluid flow. **Checkerboard instability** arises in central difference scheme due to the first derivative being zero (the central difference scheme skips the value at the central node in its first derivative $\delta f(x) = f(x + h/2) - f (x - h/2)$). This zero gradient in the first derivative makes a rapidly varying field as being uniform. A solution to checkerboard instability is to use a staggered grid. The Navier-Stokes equation is non-linear due to the advection term and is solved by iterative "guesses." 

*Advantages*: 
- FD is simple

*Disadvantages:*
- FD is not conservative, or quantities are not precisely conserved
- Subjected to false diffusivity. Numerical errors cause diffusion even in a pure advection scheme. 
- Issues with complex geometries which do not conform to the grid itself.

## Finite Volume Method
FVM does not divide the space into a regular grid but subdivides the simulated volume $V$ into many smaller volumes $v_i$ (which may have different sizes and shapes), allowing for a better representation of the shape. FVM is designed to solve conservation equations. Sources and sinks of a quantity within the volume are balanced by that quantity's flux across the volume's boundary. The concept of *divergence theorem* is central to FV. The integral over the volume is split into a sum of integrals over the finite volumes and their surface. FVM is second-order accurate using linear approximation on the surfaces and edges from all adjacent volumes. 

*Advantages*: 
- Conservative equations are satisfied
- Can model complex geometries

*Disadvantages:*
- Making approximation of complex grid is difficult
- Higher-order FV is not straightforward

## Finite Element Method
Partial Differential Equations (PDEs) are solved using the integral form - weak form, where the PDE is multiplied with a weight function $w(x)$ and integrated over the domain of interest. 

*Advantages*: 
- Can solve complex geometries with unstructured mesh and higher-order functions

*Disadvantages:*
- Does not satisfy conservation equations by default
- Checkerboard instability may occur

## Particle-Based Methods
Particle-based methods do not directly discretize the equations of fluid mechanics. Instead, these methods represent fluid using particles (which may represent an atom, a molecule, a collection of molecules, or a portion of macroscopic fluid).

### Molecular Dynamics
Molecular Dynamics (MD) solves the interaction between molecules using the intermolecular forces and tracks particles' position by calculating the acceleration as per Newton's law. However, this method is intractable to solve continuum-scale problems; even a single gram of water has $10^22$ molecules.