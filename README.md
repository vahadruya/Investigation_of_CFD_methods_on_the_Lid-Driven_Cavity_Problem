# Investigation of CFD methods on the Lid-Driven Cavity Problem

The Lid-Driven Cavity problem is studied for investigation of different basic CFD methods, namely the
1. Stream-Function Vorticity approach
2. Primitive Variable FDM
3. Finite Volume approach

## Observations

1. The SFV approach is quite easy to apply in 2D and steady incompressible constant-density flows. But it becomes tedious in 3 dimensions, and moreover, the boundary conditions are not easily derivable or intuitive as well. Hence resorting to the primitive variable approach generally is better
2. The FDM approach while much easier to implement and understand, suffers from non-divergence-free issue. Particularly because of its natural discretization of the pressure laplacian term, it creates mass imbalance and does not satisfy the continuity inherently. Furthermore, it also leads to the pressure/velocity checkerboarding problem. Hence a staggered grid for x/y momemntum with respect to the scalar grids is better suited for the primitive variable approach.
3. Since the control volumes are defined with fluxes in their surfaces, and the governing differential equations are integrated over the cell, mass conservation is inherent within each cell and the staggered grid also comes naturally in it. The divergence error which arises is of the order of the set tolerance limit for iterative schemes used to solve the pressure-poisson equation.
4. The discretization and nomenclature for FVM can get a bit complicated on implementation. Also, it takes significantly more time to obtain solutions compared to the FDM. Utilisation of sparse matrices and storage can significantly bring the order of the count of calculations and thus reduce the simulation time.

<div align = "right">    
  <a href="#investigation-of-cfd-methods-on-the-lid-driven-cavity-problem">(back to top)</a>
</div>

## Libraries Used

<a href="https://numpy.org/" target="_blank"><img src="https://img.shields.io/badge/NumPy-4d77cf?style=flat-square&logo=Numpy&logoColor=white&link=https://numpy.org/" alt="Numpy" width="84" height="25"></a>
<a href="https://matplotlib.org/" target="_blank"><img src="https://img.shields.io/badge/Matplotlib-afc6d3?style=flat-square&logo=matplotlib&logoColor=white&link=https://matplotlib.org/" alt="Matplotlib" width="78" height="25"></a>
</div>

<div align = "right">    
  <a href="#investigation-of-cfd-methods-on-the-lid-driven-cavity-problem">(back to top)</a>
</div>


## Contact

<a href="https://www.linkedin.com/in/aditya-a-p-507b1b239/" target="_blank"><img src="https://img.shields.io/badge/Linkedin-0078b7?style=flat-square&logo=linkedin&logoColor=white&link=https://www.linkedin.com/" alt="Linkedin" width="85" height="25"></a>
<a href="mailto:apaditya96@gmail.com" target="_blank"><img src="https://img.shields.io/badge/Gmail-red?style=flat-square&logo=Gmail&logoColor=white" alt="Gmail" width="70" height="25"></a>
  
<div align = "right">    
  <a href="#investigation-of-cfd-methods-on-the-lid-driven-cavity-problem">(back to top)</a>
</div>


