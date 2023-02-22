---
title: Optimization II - Acceleration in Gradient Descent
date: 2023-02-06 21:36:16
tags: optimization, gradient methods, acceleration
categories: math
mathjax: true
---

# Perspectives on Accelerated Gradient Descents

Last post we talk about several different perspectives on gradient descent. It seems that gradient descent(GD) is a fairly useful algorithm to numerically solve optimization problem. However, things may become impractical when it comes to reality. One major problem is the convergence rate.

<!-- more -->

The gradient method is strictly decreasing. The nuw value of $f$ is always less or equal to the previous value. 
$$
f(x_{k+1}) - f(x_k) \le 0
$$
However, this property is not necessary for an iteration algorithm to converge to its minimum. So what's the limits of first order algorithm? It has been proved by Nesterov that the best convergence rate of first order algorithm on a function $f$ is $O(\frac{1}{k^2})$. Here I omit some parameters depending on the smoothness of function $f$. If you are more interested in the details, please refer to [Wibisono's Blog](http://awibisono.github.io/2016/06/20/accelerated-gradient-descent.html).


To improve the convergence rate of GD, mathematicians have come up with several acceleration descent algorithms. The heave-ball and Nesterov accelaration are the best among all these algorithms.

**The Heavy-Ball method(hv-ball):**
$$
\begin{align*}
    x_{k+1} = x_k + \alpha (x_k - x_{k-1}) - s\nabla f(x_k)
\end{align*}
$$


**Nesterov method: NAG-c**
$$
\begin{align*}
    y_{k+1} &= x_k - s\nabla f(x_k)
    \\
    x_{k+1} &= y_{k+1} + \frac{k}{k+3} (y_{k+1} - y_{k})
\end{align*}
$$


**Nesterov method: NAG-sc**
$$
\begin{align*}
    y_{k+1} &= x_k - s\nabla f(x_k)
    \\
    x_{k+1} &= y_{k+1} + \frac{1-\sqrt{\mu s}}{1+\sqrt{\mu s}} (y_{k+1} - y_{k})
\end{align*}
$$

Still today researchers are studying about why these methods, with 'carefully' chosen parameters, can achieve acceleration than GD. In this post, I would like to introduce some excellent works done by researchers trying to find the secret behind first order gradient acceleraition. 


## Polynomial View
I know the subtitle - "polynomial view" may seem confusing. But it does happen that the form of acceleration descent can be derived by Chebyshev polynomials. This work is shown in [Hardt's Blog](http://blog.mrtz.org/2013/09/07/the-zen-of-gradient-descent.html)

In this blog, it shows us that the fastest first order gradient method to minimize a quadratic optimization problem can be derived by chebyshev polymoial.
$$
\min f(x) = \frac{1}{2}x^TAx -bx
$$
In quadratic form question, we can write $x_k$ explictly as a function operating on $x_0$. Then we try to find the best funtion to minimize the residual between optimal and $x_k$. After proper calculation, it turns out the Chebyshev polynomials just fits all requirements. Finally, the acceleration form is derived as 
$$
x_{k+1} = x_k - \alpha_{k+1} \nabla f(x_k) + \beta_{k+1} \nabla f(x_{k-1}).
$$

My explanation here is rough and careless. Please refer to original blog for detailed information.

## Dual Coupling View
In this subsection, I would like to introduce works that interpretes acceleration phenomenon as the linear coupling of gradient descent and mirror descent. This work is done in [Linear Coupling: An Ultimate Unification of Gradient and Mirror Descent](https://arxiv.org/pdf/1407.1537.pdf]).

In this paper, it mentions the thought under this coupling. For each step, the descent direction is closely related to the magnitute of $||\nabla f(x)||_2$. When this entity is large, we conduct gradient descent. When it is small, we conduct mirror descent. Under this thought, Zhu combine two descent methods and achieve the convergence rate of Nesterov method.

## Variational View
A variational perspective to unify gradient methods as the Euler-Lagrange equation of a Lagrangian is done by Andre Wibisono in paper [A variational perspective on accelerated methods in optimization](https://www.pnas.org/doi/pdf/10.1073/pnas.1614734113).

In this paper, it proposes a Bregman Lagrangian. By calculating the stationary point of this functional, we can get a second order ODE with multiple parameters. Then as we choose these parameters properly, we can derive Nesterov, gradient descent and several other methods.

$$
\mathcal{L}(X,V,t) = e^{\alpha_t + \gamma_t}(D_h(X+e^{\alpha_t}V, X) - e^{\beta_t}f(X))
$$


## Dynamics View
The study of acceleration dynamics are hot. These papers have proposed several different differential equations and provide their explanations. 

**Paper I**
[A Differential Equation for Modeling Nesterov's Accelerated Gradient Method: Theory and Insights](https://arxiv.org/pdf/1503.01243.pdf)

This paper raises the differential equation for NAG-c as
$$
 \ddot{X} + \frac{3}{t}\dot{X} + \nabla f(X) = 0
$$
It explains the special status of constant "3" in the equation. Also, this work inspires the variational view on Nesterov acceleration.


**Paper II**
[A Dynamical Systems Perspective on Nesterov Acceleration](https://arxiv.org/pdf/1905.07436.pdf)

This paper raises the differential equation for Nesterov acceleration as
$$
\ddot{X} + 2d\dot{X} + \frac{1}{L\gamma^2}\nabla f(X + \beta \dot{X}) = 0
$$

**Paper III**
[Understanding the acceleration phenomenon via high-resolution differential equations](https://link.springer.com/article/10.1007/s10107-021-01681-8)

This paper studies the difference between hv-ball, NAG-sc, NAG-c. Compared to previous two papers, it has reverse $x$ and $y$ value pairs. There is an extra "gradient correction" term in NAG methods to achieve more acceleration.

## What's More?

Recommend to read:

> [Wibisono's Blog](http://awibisono.github.io/)
> 
> [Hardt's Blog](http://blog.mrtz.org/2013/09/07/the-zen-of-gradient-descent.html)
