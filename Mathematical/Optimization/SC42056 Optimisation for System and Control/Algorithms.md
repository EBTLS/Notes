# Algorithms

[toc]

# 1. Linear

## 1.1. Simplex Method

1. Do not need multi-start

2. no stopping criterion is required:

   Since the simplex algorithm finds the optimal solution in a finite number of steps, no stopping criterion is required.

   However, in practice a maximum number of iterations is usually specified.

## 1.2. Interior Points

When Variable is too much

# 2. Quadratic

## 2.1. Simplified Simplex Method

only can be used when objective function is convex and quadratic

1. do not need multi-start

2. no stopping criterion is required:

   Since the modified simplex algorithm finds the optimal solution in a finite number of steps, no stopping criterion is required.

   However, in practice a maximum number of iterations is usually specified.

## 2.2. Interior Point



## 2.3. Ellipsoid Algorithm



# 3. NLP without Constraints

## 3.1. Newton

The gradient and Hessian Are needed

### 3.1.1. Original

The gradient and the Hessian of the objective function should easily be computed analytically.

1. need multi-start
2. stopping criterion

### 3.1.2. Levenberg-Marquardt algorithm

More general Newton that is able to jump out singularity

1. need multi-start

2. stopping criterion:
   $$
   ||\bigtriangledown f(x_k)||_2 \leq \epsilon \\
   \text{where $x_k$ is the current iteration point and $f$ is the objective function.}
   $$
   

### 3.1.3. Quasi-Newton (BFGS/DFP)

Improve the speed of Newton Method if we need computed gradient/ Hessian numericially

1. need multi-start

2. stopping criterion (waiting test)
   $$
   ||\bigtriangledown f(x_k)||_2 \leq \epsilon \\\text{where $x_k$ is the current iteration point and $f$ is the objective function.}
   $$

   

### 3.1.4. Attention

The Levenberg-Marquardt algorithm is to be preferred above the BFGS quasi-Newton method since the
Levenberg-Marquardt algorithm uses the exact Hessian (i.e., 2nd-order information) whenever possible, whereas
the BFGS method uses an approximation of the Hessian based on function values and gradients (i.e., 1st-order
information  



If a choice has to be made between SQP and a penalty function approach with Levenberg-Marquardt - asusming that the Hessian can be computed easily, then SQP is in general preferred as both algorithms use second-order information but SQP can deal more accurately with the constraint.

## 3.2. Direction Determination+ Line minimization

### 3.2.1. Steepest Descent Search （Gradient ）

The gradient of the objective function should be computed easily

1. need Multi-start
2. Stopping criterion:
   $$
   ||\bigtriangledown f(x_k)||_2 \leq \epsilon \\\text{where $x_k$ is the current iteration point and $f$ is the objective function.}
   $$
   
3. 

**Attention**: Because Steepest Descent Search only use Gradient but not Hessian, so when the gradient and Hessian are both easily to be computed, we prefer methods that take Hessian into consideration(like DFS)

### 3.2.2. Conjugate-Gradient Methods  

Direction is similar with Newton

1. Need Multi-Start
2. Stopping Criterion (need test)
   $$
   ||\bigtriangledown f(x_k)||_2 \leq \epsilon \\\text{where $x_k$ is the current iteration point and $f$ is the objective function.}
   $$
   

### 3.2.3. Perpendicular Search Methods

Do not need gradient and Hessian

1. Need Multi-Start
2. 

### 3.2.4. Powell's perpendicular

Do not need gradient and Hessian

1. Need Multi-Start



### 3.2.5. Parabolic Interpolation



### 3.2.6. Golden Section



### 3.2.7. Fibonacci method  

![Fibonnaci.png](.\img\Fibonnaci.png)





## 3.3. Nelder-Meld

Do not need gradient and hessian

Method is less efficient than previous methods if more than 3–4
variables  

1. need multi-start

2. stopping criterion:
   $$
   |f(x_{k+1}-f_(x_k)|\leq \epsilon_f \\
   ||x_{k+1} -x_{k}||_2 \leq \epsilon_x \\
   \text{where f is the objective function of the unconstrained problem (so if has penalty it also includes the penalty term)}
   $$
   

## 3.4. Newton Least Square

![Newton_Least_Square.png](.\img\Newton_Least_Square.png)

# 4. NLP with Constraints

## 4.1. Linear Equality (Elimination)



## 4.2. Nonlinear Equality (Lagrange)

When it is not possible to use the constraint to eliminate one of the variables

1. Stop Criterion
   $$
   ||\bigtriangledown f(x_k) + \bigtriangledown h(x_k) \lambda_k ||_2 \leq \epsilon_1 \\
   || h(x_k)||_2 \leq \epsilon_2 \\
   \text{where $x_k$ is the current iteration point}\\
   \text{$f$ is the objective of the minimization problem}\\
   \text{$h$ is the equality constraint function (when the constraint is written in the form $h(x) = 0$)}
   \text{ $\epsilon_1 , \epsilon_2$ are small positive numbers}
   $$

## 4.3. Linear Inequality(Gradient Projection/SQP)

### 4.3.1. Gradient Projection Method

Need Gradient

1. 

**Attention**: When SQP and Gradient Projection Method are both OK, use SQP Method, because the Hessian can fast the calculation



## 4.4. Nonlinear Inequality (SQP/Barrier/Penalty)

For Barrier and Penalty, if use search method
$$
\text{the $f$ in stopping criterion should change to $f_{total}$}
$$
OR use KKT

![KKT](.\img\KKT.png)

It depends on the inequality, if is easy to get gradient, then KKT, if not, f_total

>  Using the stopping criterion based on KKT is actually better than the zero gradient condition due to two reasons: the penalty function weight might not be appropriate and the penalty function approach could yield an infeasible solution. By adjusting the weight and adopting the KKT-based stopping criterion a better solution can be obtained.



## 4.5. SQP

Need Hessian and Gradient

1. need multi-start

2. stopping criterion: KKT

   ![KKT](.\img\KKT.PNG)

   

   

# 5. Convex

## 5.1. Cutting Plane Method

1. Stopping Criterion:
   $$
   |f(x_k)-f(x^*)| \leq \epsilon_f \\
   g(x_k) \leq \epsilon_g \\
   \text{where $x_k$ is the current iteration point} \\ 
   \text{$x^∗$ is the (yet unknown) optimum of this optimization problem} \\
   \text{f denotes the objective function}\\
   \text{g denotes the constraint function g}\\
   $$
   

## 5.2. Ellipsoid Algorithm

1. Stopping Criterion
   $$
   |f(x_k)-f(x^*)| \leq \epsilon_f \\
   g(x_k) \leq \epsilon_g \\
   ||x^*-x_k||_2 \leq \epsilon_{x} \\
   \text{where $x_k$ is the current iteration point} \\ \text{$x^∗$ is the (yet unknown) optimum of this optimization problem} \\\text{f denotes the objective function}\\\text{g denotes the constraint function g}\\
   $$

2. 

## 5.3. Interior Points

1. stopping criterion\
   $$
   |f(x_k)-f(x^*)| \leq \epsilon
   $$
   

# 6. Global Optimal

## 6.1. Stopping Criterion

![global_optimization_stop](\img\Global_Optimization_stoppping_criterion.png)

## 6.2. Genetic Algorithm

A slight disadvantage of the basic version of the genetic algorithm is that it requires the feasible set to be discretized, but that can be overcome in more advanced versions of the genetic algorithm.