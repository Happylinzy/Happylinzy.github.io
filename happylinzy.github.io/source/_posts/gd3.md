---
title: Optimization III - Statistical Manifold and Natural Gradient Descent
date: 2023-02-13 21:21:08
tags: optimization, gradient methods, information geometry
categories: math
mathjax: true
---
# Perspectives on Natural Gradient Descent
In our article [Optimization I - Gradient Descent](https://happylinzy.github.io/2023/02/04/gd1/#more), we talk about how to view the gradient descent as the approximation of Riemannian gradient descent.
However, is there a descent method more loylal to the Riemannian gradient descent? Or in other word, better than gradient descent in some cases. In this artile, we will start our journey to the beautiful
and attracting information geometry world. In that abstract and fancy world, the secrete equivalence between several gradient descent methods are discovered.

<!-- more -->

## KL-Divergence 
Given two distributions $p( y \mid x), p(y \mid x')$, the KL divergence measures the dsitance between these two distrbutions.
$$\begin{align*}
KL(p( y \mid x), p(y \mid x')) &= \mathbb{E}[\log (p(y \mid x))] - \mathbb{E}[\log (p(y \mid x'))]
\\
&= \sum_{y} p(y \mid x) \log (\frac{p( y \mid x)}{p(y \mid x')})
\end{align*}$$
There have been multiple extra information about KL divergence, one can refer to [wiki](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) or [Jake Tae's Blog](https://jaketae.github.io/study/natural-gradient/#:~:text=Due%20to%20the%20definition%20of%20entropy%2C%20KL%20divergence,likelihood%20is%20the%20fundamental%20bridge%20connecting%20the%20two.) for more information.

From the formulation of KL divergence we can conclude that
$$
\nabla_{x'} ^2 KL(p( y \mid x), p(y \mid x')) = -\mathbb{E}[\nabla ^2 \log (p(y \mid x'))] = F.
$$
Here $F$ is the Fisher matrix. The connection between $F$ and $KL$ also exists in the following equation:
$$
KL(p( y \mid x), p(y \mid x+\epsilon)) \approx \frac{1}{2} \epsilon^T F \epsilon.
$$
Actually this equation tells us that in a manifold where we measure the distance using KL-divergemce, the Riemanian
metric is $F$.


## Proximal View with Generalized Distance -- "Divergence"
$$\begin{align*}
\min \ &f(x_k) + \langle \nabla f(x_k), (x - x_k) \rangle
\\
s.t. \ \ &x \in \mathcal{M}
\\
&x \in B_\epsilon(x_k)
\end{align*}$$

Recall that we used to derive the gradient descent(GD) from above problem. Here how we decide the bbll $B_\epsilon(x_k)$ defined at $x_k$ is concernable. In our [first post](https://happylinzy.github.io/2023/02/04/gd1/#more) we use $l_2$ norm to measure the distance. But selecting a norm without considering the real meaning behind points is suspicious. What if the point representing a distribution? In this case, a better way to compare the distance between points is the divergence. 

Divergence is a generalized distance. Particularly here we use the Bregman divergence. Given function convex $h \in C^3$, Bregman divergence $B_h$ is defined as:
$$
B_h(x, y) = h(y) - h(x) - \langle \nabla h(x), y-x \rangle.
$$
Intuitively, you can understand the Bragman divergence as the distance between $h(y)$ and the $L_x(y)$, where $L_x(y)$ is the linear approximatio of $h(x)$ at point $x$. Click [wikipedia](https://en.wikipedia.org/wiki/Bregman_divergence#/media/File:Bregman_divergence_diagram_used_in_proof_of_squared_generalized_Euclidean_distances.png) to see the picture.

To save our time, I will not repeat the procedure to derive the formula. Here I simply provide the result
$$
x_{k+1} = x_k - \nabla^2 h(x_k)^{-1} f(x_k)
$$

Further, if we consider the manifold of point as a statistical manifold where each point represents a parameter distribution, then we can use **KL divergence** as an instance of Bregman divergence by taking $h(x) = \sum_{i}x_i\log x_i$. Now the equation is 
$$
x_{k+1} = x_k  - F(x_k)^{-1}f(x_k).
$$
Here the function $F$ is fisher matrix.

## What's More?
Recommend to read

> [Jake Tae's Blog](https://jaketae.github.io/study/natural-gradient/#:~:text=Due%20to%20the%20definition%20of%20entropy%2C%20KL%20divergence,likelihood%20is%20the%20fundamental%20bridge%20connecting%20the%20two.)