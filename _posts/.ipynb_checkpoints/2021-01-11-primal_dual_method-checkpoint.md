---
title: "Interior Points Methods - Primal Dual Method"
date: 2021-01-10 00:00:00 -0400
category : algorithms
permalink : /categories/algorithms/primal_dual
layout : single
use_math: true
comments: true
---

# Before Reading...
___
1. Here, we investigate another Interior Points Method(IPM), a primal-dual method.
2. Primal-Dual is similar to [barrier method](/categories/algorithms/barrier).
3. However, unlike barrier method, 
+ only one loop exists
  + Recall that barrier methods has nested loops, outer loop updates central path and inner loop solves central problem
+  primal and dual iterates need not to be feasible
4. **Why** do we use this method?
+ Primal-Dual method is often more efficient than Barrier method(Better than linear convergence)
+ For basic problem - LP, QP, SOCP, SDP, GP - customized primal-dual beats barrier method.
+ It works when problem is (not strictly) feasible.

# Search Direction

>## Modified KKT Condition

1. Think of (modified) KKT condition for general centering problem:
$$
\begin{align}
(\text{when }f_{i}:\mathbb{R}^{n}\rightarrow \mathbb{R})\; \nabla f_{0}(x)+\sum_{i=1}^{m}[\lambda_{i}\nabla f_{i}(x)]+A^\intercal \nu&=0 \\
-\lambda_{i}f_{i}(x)-\frac{1}{t}&=0 \nonumber \\
Ax-b&=0 \nonumber \\
(\text{when }f_{i}:\mathbb{R}^{n}\rightarrow \mathbb{R}^{m})\;\nabla f_{0}(x)+Df(x)^\intercal \lambda + A^\intercal \nu &=0
\\
-\textbf{diag}(\lambda)f(x)-\frac{1}{t}\mathbf{1}&=0 \nonumber \\
Ax-b &=0 \nonumber \\
\Bigg( \text{where } Df(x)&=\begin{bmatrix}
\nabla f_{1}(x)^\intercal \\
\cdots \\
\nabla f_{m}(x)^\intercal
\end{bmatrix}
\Bigg) \nonumber
\end{align}
$$
+ Interpretation : It is *modified* in terms of solving KKT equations for centering problem with logarithmic barrier.
  + When those system of linear equation is feasible, we get primal feasible, dual feasible, gradient optimality condition with imperfect complementarity condition
  
>## Primal-Dual Search Direction

1. Then define residual $r_{t}(x,\lambda,\nu)$ as:
$$
\begin{equation}
r_{t}(x,\lambda,\nu) = \begin{bmatrix}
\nabla f_{0}(x)+Df(x)^\intercal \lambda + A^\intercal \nu \\
-\textbf{diag}(\lambda)f(x) - \frac{1}{t}\mathbf{1} \\
Ax-b
\end{bmatrix}
\end{equation}
$$
+ If $r_{t}(x,\lambda,\nu)=0$, then 
$$x=x^{*}(t),\lambda=\lambda^{*}(t),\nu=\nu^{*}(t)$$
  + $r_{pri}=Ax-b=0$ : Primal feasible
  + $r_{dual}=\nabla f_{0}(x)+Df(x)^\intercal \lambda + A^\intercal \nu=0$ : Dual feasible
    (Why it is dual residual? Because Dual becomes infeasible unless above condition becomes zero)
  + $r_{cent}=-\textbf{diag}(\lambda)f(x)-\frac{1}{t}\mathbf{1}=0$ : Modified complementary slackness
  
2. Recall that newton step is the solution of root finding via first-order approximation:
$$
\begin{align}
y:=(x,\lambda,\nu),\;\Delta y:=(\Delta x,\Delta \lambda, \Delta \nu) \\
r_{t}(y+\Delta y) \approxeq r_{t}(y)+Dr_{t}(y)\Delta y =0 \nonumber \\ 
\Delta y=\Delta y_{nt}=-Dr_{t}(y)^{-1}r_{t}(y)
\end{align}
$$

3. Rewriting (5) with (3) generates following system:
$$
\begin{align}
\begin{bmatrix}
\nabla^{2}f_{0}(x)+\sum_{i=1}^{m} \lambda_{i}\nabla^{2}f_{i}(x) & Df(x)^\intercal & A^\intercal \\
-\textbf{diag}(\lambda)Df(x) & -\textbf{diag}(f(x)) & 0 \\
A & 0 & 0
\end{bmatrix}
\begin{bmatrix}
\Delta x \\ \Delta \lambda \\ \Delta \nu
\end{bmatrix}
= -
\begin{bmatrix}
r_{dual} \\ r_{cent} \\ r_{pri}
\end{bmatrix}
\end{align}
$$

4. Finally, the solution of (6) is defined as *primal-dual search direction* $\Delta y_{pd}=(\Delta x_{pd},\Delta \lambda_{pd},\Delta \nu_{pd})$

# Duality Gap

>## Surrogate Duality Gap

1. In primal-dual IPM, $x^{(k)},\lambda^{(k)},\nu^{(k)}$ can be infeasible
+ Of course the limit should be feasible
2. Then, unlike barrier method, we cannot evaluate a duality gap, $\eta^{(k)}$ 
3. Instead, we use *surrogate duality gap*.
+ For any $x\;s.t.\;f(x)\prec 0 \; \&  \; \lambda \succeq 0,$
$$
\begin{equation}
\hat{\eta}(x,\lambda)=-\lambda^\intercal f(x)
\end{equation}
$$
4. The surrogate gap would be *duality gap* if primal, dual feasible, *i.e.,* $r_{pri}=r_{dual}=0$
+ And when optimality, by complementary slackness, this becomes zero
5. Value of $t$ corresponding to the surrogate gap is $\frac{m}{\hat{\eta}}$

# Algorithm of Primal-Dual Method
>## Algorithm
**given** $x\;s.t.\;f_{1}(x)<0,\cdots,f_{m}(x)<0,\;\lambda \succ 0, \mu >1, \epsilon_{feas}>0,\epsilon>0$  
**repeat**
1. *Determine* t. Set
$ t:=\mu \frac{m}{\hat{\eta}}$
2. *Compute* search direction $\Delta y_{pd}$
3. *Line search* and *update*
+ Determine step lenth $s>0$ and set
$
y:=y+s \Delta y_{pd}
$

+ The algorithm terminates when achieving primal, dual feasible, and surrogate gap smaller than tolerance

>## Line Search

1. The line search is a standard backtracking line search, based on the *norm* of residual
2. Let
$$
\begin{equation}
x^{+}=x+s\Delta x_{pd},\;\lambda^{+}=\lambda+s\Delta \lambda_{pd},\;\nu^{+}=\nu + s \Delta \nu_{pd}
\end{equation}
$$
3. First, compute the largest possible step length $s$ that makes $\lambda^{+}\succeq 0$:
$$\begin{align}
s^{max} &= \sup \{ s\in [0,1] | \lambda + s \Delta \lambda \succeq 0   \} \\
&= \min \{ 1, \min [-\lambda_{i}/\Delta \lambda_{i}|\Delta \lambda_{i}<0  ]  \}
\end{align}
$$
4. Then, multiply $s$ by $\beta \in (0,1)$ until
$$
\begin{align}
f(x^{+})&\prec 0 \\
||r_{t}(x^{+},\lambda^{+},\nu^{+})||_{2} &\leq (1-\alpha s)||r_{t}(x,\lambda,\nu)||_{2}
\end{align}
$$
+ (11) corresponding to primal feasible
+ (12) corresponding to exit condition of backtracking line search


# Similarity with Barrier Method
>## Similar Search Directions

1. Search directions of primal-dual and barrier methods are closely related
2. From (6), we eliminate variable $\Delta \lambda_{pd}$ by just rewriting
$$
\begin{equation}
\Delta \lambda_{pd} = -\textbf{diag}(f(x))^{-1}\textbf{diag}(\lambda)Df(x)\Delta x_{pd} + \textbf{diag}(f(x))^{-1}r_{cent}
\end{equation}
$$
3. Use (13) to the first block of (6) gives
$$
\begin{align}
\begin{bmatrix}
H_{pd} & A^\intercal \\
A & 0
\end{bmatrix}
\begin{bmatrix}
\Delta x_{pd}\\
\Delta \nu_{pd}\\
\end{bmatrix}
&= - \begin{bmatrix}
r_{dual}+Df(x)^\intercal \textbf{diag}(f(x))^{-1}r_{cent} \\
r_{pri}
\end{bmatrix} \nonumber
\\
&=- \begin{bmatrix}
\nabla f_{0}(x)+\frac{1}{t}\sum_{i=1}^{m} \frac{1}{-f_{i}(x)}\nabla f_{i}(x)+A^\intercal \nu \\
r_{pri}
\end{bmatrix}
\end{align}
$$
+ where
$$
\begin{equation}
H_{pd} = \nabla^{2}f_{0}(x)+\sum_{i=1}^{m}\lambda_{i}\nabla^{2}f_{i}(x)+\sum_{i=1}^{m}\frac{\lambda_{i}}{-f_{i}(x)}\nabla f_{i}(x) \nabla f_{i}(x)^\intercal
\end{equation}
$$

>## Search direction of barrier method

1. Let's see the newton step in the barrier method : Newton steps for the centering problem is the solution of
$$
\begin{align}
\begin{bmatrix}
H_{bar} & A^\intercal \\
A & 0
\end{bmatrix}
\begin{bmatrix}
\Delta x_{bar}\\
\nu_{bar}
\end{bmatrix}
&=
\begin{bmatrix}
t\nabla^{2}f_{0}(x)+\nabla^{2}\phi(x) & A^\intercal \\
A & 0
\end{bmatrix}
\begin{bmatrix}
\Delta x_{nt}\\
\nu_{nt}
\end{bmatrix}
\\
&= - \begin{bmatrix}
t\nabla f_{0}(x)+\nabla \phi(x)\\
r_{pri}
\end{bmatrix} \nonumber
\\
&=-\begin{bmatrix}
t\nabla f_{0}(x)+\sum_{i=1}^{m}\frac{1}{-f_{i}(x)}\nabla f_{i}(x) \\
r_{pri}
\end{bmatrix}
\end{align}
$$
+ Note that the solutions of following system can also be interpretated as Netwon steps for solving modified KKT equations (detail skipped):
$$
\begin{align*}
\nabla f_{0}(x) + \sum_{i=1}^{m} \lambda_{i}\nabla f_{i}(x)+A^\intercal \nu &=0 \\
-\lambda_{i}f_{i}(x) &= \frac{1}{t} \\
Ax &=b
\end{align*}
$$
2. Let's compare the hessian of barrier and primal-dual KKT equations
$$
\begin{align}
H_{bar} &= t\nabla^{2}f_{0}(x)+\sum_{i=1}^{m} \frac{1}{-f_{i}(x)}\nabla^{2}f_{i}(x) + \sum_{i=1}^{m} \frac{1}{f_{i}(x)}^{2}\nabla f_{i}(x)\nabla f_{i}(x)^\intercal \nonumber
\\
(1/t)H_{bar} &= \nabla^{2}f_{0}(x)+\sum_{i=1}^{m} \frac{1}{-tf_{i}(x)}\nabla^{2}f_{i}(x) + \sum_{i=1}^{m} \frac{1}{tf_{i}(x)}^{2}\nabla f_{i}(x)\nabla f_{i}(x)^\intercal
\\
H_{pd} &= \nabla^{2}f_{0}(x)+\sum_{i=1}^{m} \lambda_{i}\nabla^{2} f_{i}(x) + \sum_{i=1}^{m} \frac{\lambda_{i}}{-f_{i}(x)}\nabla f_{i}(x) \nabla f_{i}(x)^\intercal
\end{align}
$$
3. If we add two assumptions,
+ $-f_{i}(x)\lambda_{i}=\frac{1}{t}$
+ $\Delta \nu_{bar} = (1/t)\nu_{bar}-\nu$
4. We obatin:
$$
\begin{align}
\begin{bmatrix}
(1/t)H_{bar} & A^\intercal \\ A & 0
\end{bmatrix}
\begin{bmatrix}
\Delta x_{bar} \\ \Delta \nu_{bar}
\end{bmatrix}
= -
\begin{bmatrix}
\Delta f_{0}(x) + (1/t)\sum_{i=1}^{m} \frac{1}{-f_{i}(x)}\nabla f_{i}(x)+A^\intercal \nu
\\
r_{pri}
\end{bmatrix}
\end{align}
$$
5. Here, the RHS is same as the RHS of (14) & Hessian $(1/t)H_{bar]=H_{pd}$
6. Therefore, in such case, the search directions concide!
7. We 