---
title: "Interior Points Methods - Barrier Method"
date: 2021-01-07 00:00:00 -0400
category : algorithms
permalink : /categories/algorithms/
layout : single
use_math: true
comments: true
---


[TOC]
# Before reading...
___
+ Here, we describe barrier methods to solve problems with generalized inequalities.
  + Generalized inequality form is essential in representing *[conic programming](/categories/conic_programming/)* problems.
  

# Problem Formulation
___

## Problem with generalized inequalities

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
3. Since we are dealing with **generalized inequality**, the logarithmic barrier should also be defined in proper cone $K_{i}$

# Generalized Logarithm for a Proper Cone
___
## Basic property
___
1. We define $\psi:\mathbb{R}^{q}\rightarrow \mathbb{R}$ is a *generalized logarithm* for K if  
+ $\psi$ is concave, closed, twice continuously differentiable, **dom** $\psi= \textbf{int} K$
+ $\nabla^{2}\psi(y)\prec 0$ for $y \in \textbf{int}K$
+ There is a constant $\theta>0$, which is the degree of $\psi$, such that for all $y \succ 0$, and all $s \succ 0$,  
$$
\begin{equation*}
\psi(sy)=\psi(y) + \theta \log s
\end{equation*}
$$
