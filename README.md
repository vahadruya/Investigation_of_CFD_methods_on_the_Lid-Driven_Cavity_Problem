# Lid-Driven Cavity CFD Using Vorticity Stream-Function Approach

A basic Python code simulating a Lid-Driven cavity problem using the Stream-Function Vorticity approach

<details>
<summary>Table of Contents</summary>

1. [Boundary Conditions](#boundary-conditions)
2. [Discretization](#discretization)
3. [Stability Criteria](#stability-criteria)
4. [Solution Algorithm](#solution-algorithm)
5. [Results](#results)
6. [Libraries Used](#libraries-used)
7. [Contact](#contact)
</details>

## Boundary Conditions

The stream function vorticity formulation works for two-dimensional incompressible flows to eliminate the pressure gradient term. As Prof. Suman Chakraborty had put it, the approach parallels "if you have a headache, cut off the head". The boundary conditions are to be respectively derived using the resulting Poisson's equation for stream-function and the advection-diffusion Vorticity transport equation, both of which form the governing differential equations for this formulation.

For the Lid-Driven cavity problem, the velocity and SF-Vorticity boundary conditions are shown below.

![download](https://github.com/vahadruya/Lid_driven_cavity_CFD_using_Vorticity_SF_approach/assets/115869753/3157e69b-d66e-4599-b757-974b5bb0454a)


<div align = "right">    
  <a href="#lid-driven-cavity-cfd-using-vorticity-stream-function-approach">(back to top)</a>
</div>

## Discretization

- Firstly, the wall vorticity boundary conditions need to be discretized. This can be performed using the Taylor series expansion for differentials. As an example, the most complex of the 4 (top wall) is shown:

$$\psi_{M-1, i} = \psi_{M, i} - \frac{\partial\psi}{\partial y}\Delta y + \frac{\partial^2\psi}{\partial y^2}\frac{(\Delta y)^2}{2}+O(\Delta y^3)$$

$$\frac{\partial^2\psi}{\partial y^2} = \frac{2}{(\Delta y)^2}\psi_{M-1, i} - \frac{2U}{\Delta y} + O(\Delta y)$$

- The Poisson governing equation for Stream-function can be discretized using the CDS of 2nd order. After re-arranging for the stream function value at the (i, j)th node:

$$\psi_{\text{i, j}}^{\text{k+1}} = \frac{\beta}{2((\Delta x)^2+(\Delta y)^2)}\left[\omega_{i, j}(\Delta x)^2(\Delta y)^2 + (\psi_{\text{i+1, j}}^{\text{k}}+\psi_{\text{i-1, j}}^{\text{k+1}})(\Delta y)^2 + (\psi_{\text{i, j+1}}^{\text{k}}+\psi_{\text{i, j-1}}^{\text{k+1}})(\Delta x)^2 \right] + (1-\beta)\psi_{\text{i, j}}^{\text{k}}$$

where the Poisson's equation is given by

$$\frac{\partial^2\psi}{\partial x^2} + \frac{\partial^2\psi}{\partial y^2} = -\omega$$

- The Vorticity transport equation is discretized using the explicit FTCS scheme in this project, resulting in some criterion for stability of the time step size and grid sizes. The vorticity transport equation, and the vorticity at (i, j)th node after using FTCS and re-arranging are given respectively as:

$$\frac{\partial\omega}{\partial t} + \frac{\partial\psi}{\partial y}\frac{\partial\omega}{\partial x} - \frac{\partial\psi}{\partial x}\frac{\partial\omega}{\partial y}= \nu\nabla^2\omega$$

$$\omega_{\text{i, j}}^{\text{n+1}} = \frac{\Delta t}{2\Delta x\Delta y}\left[\left(\psi_{\text{i+1, j}}^{\text{n}} - \psi_{\text{i-1, j}}^{\text{n}}\right)\left(\omega_{\text{i, j+1}}^{\text{n}} - \omega_{\text{i, j-1}}^{\text{n}}\right) - \left(\psi_{\text{i, j+1}}^{\text{n}} - \psi_{\text{i, j-1}}^{\text{n}}\right)\left(\omega_{\text{i+1, j}}^{\text{n}} - \omega_{\text{i-1, j}}^{\text{n}}\right)\right] + \nu\Delta t\left[\frac{\omega_{\text{i+1, j}}^{\text{n}}+\omega_{\text{i-1, j}}^{\text{n}}}{(\Delta x)^2}+\frac{\omega_{\text{i, j+1}}^{\text{n}}+\omega_{\text{i, j-1}}^{\text{n}}}{(\Delta y)^2}\right]$$

$$ + \omega_{\text{i, j}}^{\text{n}}\left[1 - 2\nu\Delta t\left(\frac{1}{(\Delta x)^2} + \frac{1}{\Delta y)^2}\right)\right]$$

<div align = "right">    
  <a href="#lid-driven-cavity-cfd-using-vorticity-stream-function-approach">(back to top)</a>
</div>

## Stability Criteria

The FTCS scheme for the advection-diffusion equation is not unconditionally stable, but has to follow a stability criteria (which can be derived using the Von-Mises analysis). For two dimensions, they are:

$$\frac{\mu\Delta t}{\Delta x^2} \le \frac{1}{4} \qquad AND \qquad \frac{(|U|^2+|V|^2)\Delta t}{\mu}\le4$$

based on which the time step size and grid sizes should be calculated.

<div align = "right">    
  <a href="#lid-driven-cavity-cfd-using-vorticity-stream-function-approach">(back to top)</a>
</div>

## Solution Algorithm

1. Initialise $\psi$ and $\omega$ over the entire domain, and compute the vorticity boundary conditions by using the 2nd order derivative relations shown in figure above. Make sure that the terms in the Taylor's series expansions are appropriately handled
2. Solve the Poisson's equation of the stream-function with the existing vorticity values to obtain the latest $\psi$ values. An inner iteration Gauss-Seidel or ADI schemes can be used, till convergence is reached.
3. Use these updated values of $\psi$ to solve the vorticity transport equation to obtain the vorticity at the new time step. If a discretization forward in time is used, then no inner iterations are required as they are explicit methods, but which comes with stability criterion as above.
4. Repeat step two and three for the new values of $\omega$, till final time step is reached.
5. The velocity field can be computed using the relations of stream functions and velocities.

<div align = "right">    
  <a href="#lid-driven-cavity-cfd-using-vorticity-stream-function-approach">(back to top)</a>
</div>

## Results

For the following settings:
- Length along x axis - 1m
- Length along y axis - 1m
- Fluid velocity at lid - 10m/s
- Total time of simulation - 1s
- Kinematic Viscosity - 0.05m2/s
- Number of grids along x and y - (33, 33)
- Convergence tolerance - 1E-3
- Over-relaxation factor - 1.5

the following stream-line progression (along equal interviews of time-steps) is obtained:

![streamline_animation](https://github.com/vahadruya/Lid_driven_cavity_CFD_using_Vorticity_SF_approach/assets/115869753/77f7ab1d-c4f3-49f2-9c4b-03e137ac9f4b)

<div align = "right">    
  <a href="#lid-driven-cavity-cfd-using-vorticity-stream-function-approach">(back to top)</a>
</div>

## Libraries Used

<a href="https://numpy.org/" target="_blank"><img src="https://img.shields.io/badge/NumPy-4d77cf?style=flat-square&logo=Numpy&logoColor=white&link=https://numpy.org/" alt="Numpy" width="84" height="25"></a>
<a href="https://matplotlib.org/" target="_blank"><img src="https://img.shields.io/badge/Matplotlib-afc6d3?style=flat-square&logo=matplotlib&logoColor=white&link=https://matplotlib.org/" alt="Matplotlib" width="78" height="25"></a>
</div>

## Contact

<a href="https://www.linkedin.com/in/aditya-a-p-507b1b239/" target="_blank"><img src="https://img.shields.io/badge/Linkedin-0078b7?style=flat-square&logo=linkedin&logoColor=white&link=https://www.linkedin.com/" alt="Linkedin" width="85" height="25"></a>
<a href="mailto:apaditya96@gmail.com" target="_blank"><img src="https://img.shields.io/badge/Gmail-red?style=flat-square&logo=Gmail&logoColor=white" alt="Gmail" width="70" height="25"></a>
  
<div align = "right">    
  <a href="#lid-driven-cavity-cfd-using-vorticity-stream-function-approach">(back to top)</a>
</div>


