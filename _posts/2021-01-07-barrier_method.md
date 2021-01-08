---
title: "Interior Points Methods - Barrier Method"
date: 2021-01-07 00:00:00 -0400
category : algorithms
permalink : /categories/algorithms/
layout : single
use_math: true
comments: true
---



# Before reading...
___
+ Here, we describe barrier methods to solve problems with generalized inequalities.
  + Generalized inequality form is essential in representing *[conic programming](/categories/conic_programming/)* problems.
+ We assume the readers are familiar with unconstrained optimization, such as *[newton method](/categories/algorithms/newton_method)* or *[gradient descent method](/categories/algorithms/gradient_descent_method)*

# Problem Formulation
___

> ## Problem with generalized inequalities

1. Consider following problem:  
+ 
$$
\begin{align}
\min \:&f_{0}(x)
\\
\text{s.t.}\: &f_{i}(x) \preceq_{K_{i}} 0,\;i=1,\cdots,m \nonumber
\\
&Ax=b \nonumber
\end{align}
$$
+ where $f_{0}:\mathbb{R}^{n}\rightarrow \mathbb{R}$ is convex, $f_{i} : \mathbb{R}^{n}\rightarrow \mathbb{R}^{k_{i}},\forall i$ are *$K_{i}$-convex*, and $K_{i}\subseteq \mathbb{R}^{k_{i}}$ are proper cones  
+ We assume $f_{i}$ are twice continuously differentiable, $A\in \mathbb{R}^{p \times n}$ with **rank** $A=p$, and that problem is solvable  

2. The *KKT* conditions for problem (1) is
+ $$
\begin{align}
&\nabla f_{0}(x^*) + \sum_{i=1}^{m} Df_{i}(x^*)^\intercal \lambda_{i}^*+A^\intercal \nu^* = 0 \\
&f_{i}(x^*) \preceq_{K_{i}} 0,\:\forall i \nonumber \\
&Ax^* = b \nonumber \\
&\lambda_{i}^* \preceq_{K_{i}^*} 0,\:\forall i \nonumber \\
&(\lambda_{i}^*)^\intercal f_{i}(x^*) = 0,\:\forall i \nonumber
\end{align}
$$  
+ where $Df_{i}(x^*) \in \mathbb{R}^{k_{i}\times n}$ is the derivative(Jacobian)
3. Of course, directly solving KKT equations is usually hard in many problems. That's why we employ **algorithms**.
+ If we remove the inequalities $f_{i}\preceq_{K_{i}}0$, then the problem can be solved by standard *newton method*.
  + What about removing the constraints and adding them to objective function with penalty? (Recall lagrangian paradigm)
+ Our hope : nonlinear constraints $f_{i}(x) \preceq_{K_{i}} 0$ never be violated.
  + Natural penalty can be thought of as logarithmic transform($-\log (-f_{i}(x))$), which goes to $\infty$ as $f_{i}(x) \rightarrow 0$ (go outside of the boundary)
  + This is the reason why we call **interior**, because the algorithm never touch the boundary, just exploring the interior of feasible region.
5. Since we are dealing with **generalized inequality**, the logarithmic barrier should also be defined in proper cone $K_{i}$

# Generalized logarithm and logarithmic barrier functions
___
> ## Generalized Logarithm for a Proper Cone

1. We define $\psi:\mathbb{R}^{q}\rightarrow \mathbb{R}$ is a *generalized logarithm* for K if  
+ $\psi$ is concave, closed, twice continuously differentiable, **dom** $\psi= \textbf{int} K$
+ $\nabla^{2}\psi(y)\prec 0$ for $y \in \textbf{int}K$
+ There is a constant $\theta>0$, which is the degree of $\psi$, such that for all $y \succ 0$, and all $s \succ 0$,
$$
\begin{equation*}
\psi(sy)=\psi(y) + \theta \log s
\end{equation*}
$$
+ If $y\succ_{K}0$, then $\nabla\psi(y)\succ_{K^*}0$
+ $y^\intercal \nabla \psi(y) = \theta $

> ## Examples

1. Nonnegative Orthant
+ $\psi(x)= \sum_{i=1}^{n}\log x_{i}$ for $K = \mathbb{R}_{+}^{n}$ with degree $n$
+ $\nabla \psi(x) = (1/x_{1},\cdots,1/x_{n})$
+ $\nabla \psi(x) \succ 0, x^\intercal \psi(x) = n$  
2. Second-order Cone(Lorentz Cone;Ice cream Cone)
+ $\psi(x) = \log (x_{n+1}^{2}-\sum_{i=1}^{n}x_{i}^{2}$
+ $\frac{\partial \psi(x)}{\partial x_{j}}= \frac{-2x_{j}}{(x_{n+1}^{2}-\sum_{i=1}^{n}x_{i}^{2})}, j=1,\cdots,n$
+ $\frac{\partial \psi(x)}{\partial x_{n+1}} = \frac{2x_{n+1}}{(x_{n+1}^{2}-\sum_{i=1}^{n}x_{i}^{2})}$
+ $\nabla \psi(x) \in \textbf{int} K^* = \textbf{int} K$
+ $x^\intercal \nabla(x)=2$
3. Positive-Semidefinite Cone
+ $\psi(x) = \log det X$ for cone $S_{+}^{p}$ with degree $p$
+ $\nabla \psi(X) = X^{-1} \succ 0$ : proof in appendix
+ $tr(XX^{-1})=p$

> ## Logarithmic barrier functions

1. We define the *logarithmic barrier function* for problem (1) as
$$
\begin{equation*}
\phi(x) = -\sum_{i=1}^{m}\psi_{i}(-f_{i}(x)),\hspace{2em} \textbf{dom}\phi = \{x|f_{i}(x) \prec 0, i=1,...,m\}
\end{equation*}
$$

2. Of course this function is convex from the fact that generalized logarithms are $K_{i}$-increasing with $K_{i}$-convex $f_{i}(x)$
+ refer composition rule


# Central Path and Barrier Method
___
> ## Central Path

1. Central path is the sequence of the central points, ${x^*(t),t>0}$, which is the minimizer of following centering problem:
+ $$
\begin{align}
\min\: &f_{0}(x)-\frac{\phi(x)}{t} \\
\text{s.t. }&Ax=b \nonumber
\end{align}
$$
+ This problem can be solved by standard *newton method*
  + Note that this is just an "approximation" of the original problem (1)
  + We need to solve this problem iteratively until we achieve optimal.
+ $t$ is the hyperparameter, controls the accuracy of the approximation
  + Large $t$ makes great approximation, good solutions, but iterations of newton method in each subproblem gets bigger
  + Therefore, we solve centering problems iteratively by growing $t$, until we get $\epsilon-$optimal
+ Theoretically, $t$ affects the number of iterations, however its influence get vanished when the $f_{i}(x)$ is *self-concordant* function.
  + Refer *[Boyd&Vandenberghe](https://web.stanford.edu/~boyd/cvxbook/)*
2. Let's modify (3) to equivalent problem,
$$
\begin{align}
\min\: &tf_{0}(x)-\phi(x) \\
\text{s.t. }&Ax=b \nonumber
\end{align}
$$

> ## Algorithm of Barrier Method
**given** strictly feasible $x, t:=t^{(0)}>0, \mu>1, \epsilon >0$  
**repeat**  
+ Solve centering problem (4).
  + Compute $x^*(t)$ by using newton method.
+ Update $x:=x^*(t)$.
+ Stopping criterion : **quit** if $\frac{\bar{\theta}}{t}<\epsilon$.
+ Increase $t$ : $t:=\mu t$  

  
> ## Dual points on central path

1. We can guess that getting the optimality gap, $\frac{\bar{\theta}}{t}$ is important
  + In fact, this is the *duality gap*!
2. Central point $x^*(t)$ is acquired by solving optimality condition:
$$
\begin{align}
&t\nabla f_{0}(x) + \nabla \phi(x) + A^\intercal \nu \nonumber \\
&=t\nabla f_{0}(x) + \sum_{i=1}^{m} Df_{i}(x)^\intercal \nabla \psi_{i}(-f_{i}(x))+A^\intercal \nu = 0
\end{align}$$  
3. Define $\nu^*(t)=\frac{\nu}{t}$
+ and  $\lambda_{i}^*(t)=\nabla\psi_{i}(-f_{i}(x^\*(t)))/t$
4. Surprisingly, $\lambda_{i}^\*(t), \nu^\*(t)$ are dual feasible solution of original problem (1) !
+ $\lambda_{i}^\*(t) \succ_{K_{i}^*} 0$
  + Dual feasibile (Property of generalized logarithm) 
+ Lagrangian $L(x,\lambda_{i}^\*(t),\nu^*(t)) = f_{0}(x)+\sum_{i=1}^{m}\lambda_{i}^\*(t)^\intercal f_{i}(x)+\nu^\*(t)^\intercal (Ax-b)$  
 is minimized over central point $x=x^\*(t)$
  + This is followed by directly using the result of (5)
+ Therefore, $\lambda_{i}^\*(t), \nu^\*(t)$ are one of dual feasible solution of (1)
5. Then, lagrangian dual function
$$
\begin{align*}
g(\lambda^*(t),\nu^*(t))&= f_{0}(x^*(t)) + \sum_{i=1}^{m} (\lambda_{i}^*(t))^\intercal f_{i}(x^{*}(t))+\nu^{*}(t)^\intercal(A(x^{*}(t))-b)
\\
&= f_{0}(x^*(t)) + (1/t)\sum_{i=1}^{m}\nabla \psi_{i}(-f_{i}(x^{*}(t)))^\intercal f_{i}(x^{*}(t))
\\
&(\text{from the fact : }y^\intercal \psi_{i}(y)=\theta_{i} (degree))
\\
&= f_{0}(x^*(t)) - (1/t)\sum_{i=1}^{m}\theta_{i}
\\
&= f_{0}(x^*(t)) - \frac{\bar{\theta}}{t}
\\
\therefore f_{0}(x^*(t))-g(\lambda^*(t),\nu^*(t)) &= \frac{\bar{\theta}}{t}
\end{align*}
$$

# Feasibility problem(Phase-1)
>## Phase 1 methods

1. Natural question:
+ What if the case in which relative interior point do not exist?
+ Even if it exsits, how can we find it?
2. Finding initial point of algorithm is itself an another optimization problem
3. Just solve the problem:
+ $$
\begin{align}
\min\:&s \\
\text{s.t. }&f_{i}(x) \preceq_{K_{i}} se_{i},\;i=1,\cdots,m \nonumber \\
& Ax=b
\end{align}
$$
+ where $e_{i}\succ_{K_{i}}0$
4. If $s^{*}\leq 0$, the problem is either infeasible or unsolvable by interior point method

# Convergence Analysis
>## Number of iterations

1. Number of outer loops(centering steps) required to get $\epsilon$-optimal = $\frac{\log(\bar{\theta}/(t^{(0)}\epsilon))}{\log \mu}+1(\text{initial step})$
+ for k step, directly solve $\mu^{k}t^{0}=\frac{\bar{\theta}}{\mu^{k}t^{0}}=\epsilon$ returns above result.
2. Number of innter loop(solving (4) by newton method)? Relies on *Lipschitz constant*, *conditional number* (detail skipped)
+ for the standard newton method, it suffices that $tf_{0}(x)-\phi(x)$, its initial sublevel set closed, inverse KKT matrix bounded, Hessian satisfiying Lipschitz condition (check the newton method)
+ Can we get more tangible results? Yes, if the constraints are *self-concordant*