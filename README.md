# 2D Elliptic Mesh Generator
This is a <b>2D orthogonal elliptic mesh generator</b> which uses the <b>Winslow (modified Laplace) partial differential equations</b>. 
It also uses <b>univariate stretching functions</b> and a <b>tilted parabola tangent line fitter</b> (original discovery). The grid generator is packaged as a Java program which can be compiled and exectuted via the command line. The program allows one to choose
from six different boundary types: rectangular, gaussian, absolute value, greatest-integer, forwards step and semi-ellipse. Then one must
specify the coordinates of the grid domain (<b><i>warning: the domain must be perfectly square</i></b>). Finally, one can choose to add refinements
to the grid, such as <b>orthogonality</b> adjustment and <b>stretching functions</b>. The program will then generate an initial course grid and iteratively refine it to produce a <b>smooth grid</b> with the given parameters and refinement options. A detailed analysis of the quality of the resulting grid will also be provided. 

## Screenshots
Here are somee examples of grids generated by the program:



A more complete collection can be found within the screenshots folder.

## Elliptic Grid Generation Algorithm
Firstly to construct an initial grid, the <b>Transfinite Interpolation algorithm</b> is applied to the given domain constrained by the
specified boundary conditions. Next, the Winslow equations are applied to the grid using the method of <b>mixed-order finite differences</b>, thereby generating a system of equations for each one dimensional line of nodes in the grid. This system of equations is then 
modeled in matrix representation, resulting in a tri-diagonal matrix. This matrix is then solved using the <b>Thomas Tri-Diagonal Matrix
Algorithm</b>. The solution to the current iteration is then further processed by the orthogonaliy adjustment algorithm and stretching
function methods as necessary. The solver then calculates the solution for all other node lines and repeats the process until the difference between adjacent nodes meets a threshold convergence criteria.

## Orthogonality Adjustment Algorithm
In several <b>computational fluid dynamics</b> applications, an orthogonal mesh is necessary in certain regions to ensure a high enough accuracy when performing calculations. However, it is not always possible to achieve a fully orthogonal solution, and thus the problem becomes finding a nearly-orthogonal solution to an arbitrarily defined domain. The implemented solution uses an iterative approach to find the angles of intersection and adjust the position of the nodes until their respective angles of intersection converge to a reasonable threshold value from 90 degrees. The exact method makes use of the <b>linear approximation</b> of the grid lines intersecting at each node within the grid. A remarkable result from the research was the development of an accurate method for obtaining these linear approximations. This method consists of fitting a tilted parabola to three adjacent nodes using <b>coordinate transformations</b>. The resulting trigonometrically transformed equation of the parabola is solved using the bisection method for the angular position of the parabola (can be improved with Newton's method). The same process is applied to the three oppositely adjacent nodes. From this, a suitable linear approximation is obtained, and the adjustment is obtained by plugging the slopes of the two linear functions into the linear equation relating the two obtained by basic analytical geometry.

## Stretching Functions
In order to further improve the quality of the grid, one can introduce <b>univariate stretching functions</b> to either compress or expand grid lines in order to correct non-uniformity where grid lines are more or less dense. These functions are arbitrarily chosen and only reflect the distribution of grid lines. Upon implementation, the Winslow system becomes a Poisson system, thereby slightly modifying the solution process by changing the values of the matrix coefficients.

## Grid Quality Analysis Report
In order to determine the quality of the resulting mesh, it was necessary to construct an objective means of quality measurement. Therefore, several <b>statistical procedures</b> were implemented in the program to produce a <b>meaningful grid quality analysis report</b>. The metrics which are presented are divided into the following categories:

* Orthogonality Metrics
  * Standard deviation of angles
  * Mean angle
  * Maximum deviation from 90 degrees
  * Percentage of angles within x degrees from 90 degrees (x can be set as a constant in the code)
  
* Cell Quality Metrics
  * Average aspect ratio of all cells
  * Standard deviation of all aspect ratios
  
In the above list, "angle" refers to the angle of intersection of grid lines at a node and "aspect ratio" refers to the skewness of a grid cell measured as a ratio of the height to the length of the cell.

## Libraries Used
* JMathPlot by Yann Richet

## Context/Acknowledgement
This project was independently written for Prof. Jonathon Sumner at Dawson College as part of a research assistant internship. A research paper detailing the mathematical results of the project is currently in the works.

