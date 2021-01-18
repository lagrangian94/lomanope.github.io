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
+ $\mathcal{T}_{1}: \;\Omega=">"\;\&\; e \geq 0 \Leftrightarrow \;d^\intercal x = 0 > e \geq 0$  
or,  
+ $\mathcal{T}_{2}: \;\Omega="\geq"\;\&\; e > 0 \Leftrightarrow \;d^\intercal x = 0 \geq e > 0$
6. Let's extend this result to $\mathcal{S}$

# General Theorem of Alternatives

>## Theorem of Alternatives
System $\mathcal{S}$ has no solutions if and only if either $\mathcal{T}$ is solvable
$$\begin{align*}
\mathcal{S}:\;\;\begin{Bmatrix} a_{i}^\intercal x &> b_{i},\;i=1,\cdots,m_{s} \\ 
a_{i}^\intercal x &\geq b_{i},\;i=m_{s}+1,\cdots,m \end{Bmatrix}  \\
\mathcal{T}_{1}:\;\;\begin{Bmatrix}
\lambda &\geq 0 \\
\sum_{i=1}^{m}\lambda_{i}a_{i} &= 0\\
\sum_{i=1}^{m}\lambda_{i}b_{i} &\geq 0 \\
\sum_{i=1}^{m_{s}} \lambda_{i} &> 0
\end{Bmatrix}
\\
&\text{or} \\
\mathcal{T}_{2}:\;\;\begin{Bmatrix}
\lambda &\geq 0 \\
\sum_{i=1}^{m} \lambda_{i}a_{i}&=0 \\
\sum_{i=1}^{m} \lambda_{i}b_{i} &>0
\end{Bmatrix}
\end{align*}
$$
+ Assume at least one of $\mathcal{T}$ is solvable. Then, the system $\mathcal{S}$ is infeasible 
+ Numerous proofs exist. To describe Duality, *Farkas Lemma* is one of the favorites.

>## (Homogenous) Farkas Lemma
A linear inequality
$$
\begin{equation*}
a^\intercal x \leq 0
\end{equation*}
$$
is a consequence of a system of linear inequalities
$$
\begin{equation*}
a_{i}^\intercal x \leq 0,\;i=1,\cdots,m
\end{equation*}
$$
**if and only if** $a$ is in the conic combination of $a_{i}$:
$$
\begin{align*}
\exists \lambda_{i}\geq 0\;s.t.\; a= \sum_{i}\lambda_{i}a_{i}
\end{align*}
$$

>## Proof of Farkas Lemma