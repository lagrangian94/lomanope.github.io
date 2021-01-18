---
title: "Duality interpreted via Theorem of Alternatives"
date: 2021-01-10 00:00:00 -0400
category : conic programming
permalink : /categories/conic programming/duality_toa
layout : single
use_math: true
comments: true
---

# Before Reading...
___
1. This is written after reading a textbook, [Lectures on Modern Convex Optimization]("https://www2.isye.gatech.edu/~nemirovs/LMCO_LN.pdf")
1. Duality theory so important in convex optimization that many interpretations exist : Lagrangian, Game-theory, Saddle-point, etc.
2. Here, we see dual problem through *Theorem of Alternatives*, which clarifies the connection between *linear Programming* and *Conic Programming*.
3. Assume that readers are familiar with concepts of Lagrange Dual, Linear Programming.


# Certificates for solvability and insolvability
>## Simple certificate of the system's feasibility

1. Somebody asked you to verify below (1) do not have a solution (infeasible). How would you do it?
$$\begin{align}
f_{i}(x)\; &\Omega_{i} \;0,\;i-1,\cdots,m \\
\Omega_{i}\;:\;&\text{either }">"\text{ or }"\geq" \nonumber
\end{align}$$
+ Suppose the question was to verify that (1) is feasible and you have a feasible solution. Then you can just put the solution and show the result.
+ However, to prove that the system has no solution(negative statement) is quite different, often difficult.

2. In some cases, there exist *simple certificates* of negative statements.
+ You can prove the infeasibility of the system (1), by demonstrating that a *consequence* of (1) is a **contradictory inequality** such as $-1 \geq 0$

3. Below nonnegative linear combination of inequality is one of the consequence of (1) ($Cons(\lambda)$)
$$
\begin{align}
Cons(\lambda):\;\;\sum_{i=1}^{m}\lambda_{i}f_{i}(x)\;\Omega \; 0
\end{align}
$$
+ If (2) has no solutions at all, then we can be sure that (1) has no solution.
+ We call nonnegative weight, $\lambda$, as a "simple certificate" of (1)

4. Let us narrow our subject to "linear" inequality.
$$
\begin{align}
\mathcal{S}\; :\; a_{i}^\intercal x \; &\Omega_{i} \; b_{i},\;i=1,\cdots,m\;  \bigg[  \Omega_{i}=\begin{Bmatrix} ">" \\ "\geq" \end{Bmatrix} \bigg] \\
Cons(\lambda)\; :\; (\sum_{i=1}^{m}\lambda a_{i})^\intercal x\; &\Omega\; \sum_{i=1}^{m}\lambda b_{i} \Leftrightarrow d^\intercal x \;\Omega \;e \nonumber
 \end{align}
$$

5. When $Cons(\lambda)$ be contradictory?
+ First, set $d=0$
+ $\Omega=">"\;\&\; e \geq 0 \Leftrightarrow \;d^\intercal x = 0 > e \geq 0$
+ $\Omega="\geq"\;\&\; e > 0 \Leftrightarrow \;d^\intercal x = 0 \geq e > 0$
+ Let's extend this result to $\mathcal{S}$
6. 