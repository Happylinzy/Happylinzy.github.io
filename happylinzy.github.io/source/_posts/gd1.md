---
title: Optimization I - Gradient Descent
date: 2023-02-04 12:30:36
tags: optimization, gradient methods
categories: math
mathjax: true
---

# Perspectives on Gradient Descent

When talking about numerical solution to optimization problems, nobody can avoid the gradient descent method
created by Cauchy in 1847. In this blog, we try to understand this first order algorithm through different perspectives.
$$
x_{k+1} = x_{k} - \alpha \nabla f(x_k)
$$

<!-- more -->

## Problem Formulation
An optimization problem is trying to find the minimum value of a given function under constraints. 
$$\begin{align*}
\min f(x)
\\
s.t. \ \ x \in \mathcal{M}
\end{align*}$$

Assumptions on $f$:

 $f$ is convex
 
 $f$ is differentiable, $f \in C^1$
 
 $f$ is Lipschitz gradient continuous

## Proximal View
Given an initial value $x_0$, it is almost impossible to explictly express optimal $x^*$ as formula of $x_0$, which means globally we don't know the relatino between $x^*$ and $x_0$. But we can find a local optimal point which is better than $x_0$. Then hopefully we can keep this process until we reach the $x^*$.

For point $x_k$, we have local approximation of $f(x)$ near $x_k$ according to Taylor series.
$$
f(x) = f(x_k) + \langle \nabla f(x_k), (x - x_k) \rangle + \frac{1}{2} \nabla^2 (x - x_k)^Tf(x_k)(x - x_k) + O(x^3)
$$

Here we take first order approximation of $f(x)$ since our assumption only guarantees that $f(x)$ is differentiable. Then we have local optimization problem as 

$$\begin{align*}
\min \ &f(x_k) + \langle \nabla f(x_k), (x - x_k) \rangle
\\
s.t. \ \ &x \in \mathcal{M}
\\
&x \in B_\epsilon(x_k)
\end{align*}$$

The constraint $x \in B_\epsilon(x_k)$ gurantees that our local optimal $x$ is local. Naturally we consider the point $x$ in Euclidean space with norm $\frac{1}{2}||\cdot||^2$. This means the $x \in B_\epsilon(x_k)$ is $||x - x_k||^2 = \epsilon$

Then the Lagrangian of above local optimization is
$$
\mathcal{L} = f(x_k) + \langle \nabla f(x_k), (x - x_k) \rangle - \beta ||x - x_k||^2 + \beta \cdot \epsilon
$$
Let $\frac{\partial \mathcal{L}}{\partial x} = 0$ we get 

$$\begin{align*}
& \nabla f(x_k) = -2\beta (x - x_k)
\\
\Rightarrow & x = x_k - \frac{1}{2\beta}\nabla f(x_k)
\end{align*}$$

We choose $\beta = \frac{1}{2\alpha}$ we can get local explict optimal solution as $x_{k+1}$.

In local proximal view, gradient descent is derived as local optimal solution in $l_2$ norm space with first order Taylor approximation.

## Geometry View
Suppose that readers are familiar with differential geometry, we can see some hidden facts in gradient descent. Rewrite the formula as 
$$
x_{k+1} - x_k = -\alpha \nabla f(x_k)
$$
The left hand side of above equation is cotravariant and right hand side is covariant (under a change of coordinate system). If we change the parametrization of $f$ from $x$ to $\eta$, the gradient descent may lead to a different optimal value. Fortunately, this problem will not happen in $l_2$ norm Euclidean space. To state the reason, we need help of differential geometry.

Consider the Riemannian manifold $\mathcal{M}$ of point $x$, the tangent space at $x$ is denoted as $T_xM$. Then a natural idea is find the steepest direction in $T_xM$ and decent $x$ along the geodesic on that direction. With the help of Riemannian gradient we have
$$
    x_{k+1} = exp_{x_k}(-\alpha \cdot grad \ f(x_k))
$$
Generaly, the exponential map is hard to calculate, where we use retraction map to approximate it.
$$
    x_{k+1} = R_{x_k}(-\alpha \cdot grad \ f(x_k))
$$
One of the retraction choice is local first order approximation $R_{x_k}(v) = x_k + v$. Therefore, we have
$$
    x_{k+1} = x_k -\alpha \cdot grad \ f(x_k)
$$
Here the Riemannian gradient is different with normal gradient. It operates on function value with a Riemannian metric $g$.
$$
    x_{k+1} = x_k -\alpha g^{-1} \nabla f(x_k)
$$
In Euclidean space with norm $l_2$, the Riemannian metric is identity matrix $g = I$. 
From a geometric view, gradient descent is a special case of Riemanniam descent in Euclidean space where Riemannan matric is identity.


## Dynamic View
In dynamic view, we try to understand gradient descent as a physical system. Consider the $x_k$ as a sample of a curve at time $t$ with step size $\delta$, which means $t = \delta k$. Then we get $X_t = X_{\delta k} = x_k$.

Then we have the Talyor series to connect $x_k$ and $x_{k+1}$.
$$
x_{k+1} = X_{\delta k + \delta} = X_{t + \delta} = X_t + \dot{X_t} \delta + \ddot{X_t}\frac{\delta^2}{2} + O(\delta^3)
$$

Plug in the form of gradient descent we have
$$
 \dot{X_t} \delta + \ddot{X_t}\frac{\delta^2}{2} + O(\delta^3) = - \alpha \nabla f(X_t)
$$
Omit $O(\delta^2)$ item, let $\delta = \alpha$ scale, we have
$$\begin{align*}
    & \dot{X} = - \nabla f(X)
\\
\Leftrightarrow &\lim_{m \rightarrow 0}  m \ddot{X} + \dot{X} + \nabla f(X) = 0
\end{align*}$$
which accounts to a massless particle damping equation. The equation can be viewed as the Euler-Lagrange equation of
Lagrangian $\mathcal{L} = e^{\frac{t}{m}}(\frac{m}{2}||\dot{X}||^2 - f(X))$


## What's More?

Recommend to read:

> [Kristiadi's Blog](https://agustinus.kristia.de/techblog/2019/02/22/optimization-riemannian-manifolds/)