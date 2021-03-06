---
layout: post
title:  "Nonparametric GLM estimation"
date:   2014-3-16
tags: notebook
---

## The theorem

These results are proved in Fan, Heckman, and Wand (1995). Let $p$ be the order of the polynomial approximation (typically, $p=1$), and $r$ be the order of derivative of the nonparametric function under estimation (typically, $r=0$.) Let $p - r \> 0$ be odd and the conditions in the appendix satisfied.

Further assume that $h=h\_n \rightarrow 0$ and $nh^3 \rightarrow \infty$ as $n \rightarrow \infty$.

If $x$ is a fixed point on the interior of the support of $f(x)$ (which is the density of the locations of $X$), then:

$$\sqrt{nh^{2r+1}}\sigma\_{r,p}(x;K)^{-1} \times \left[ \hat{\eta}\_r (x;p,h) - \eta^{(r)}(x) - \left\\{ \int z^{p+1} K\_{r,p}(z) dz \right\\} \left\\{\frac{\eta^{(p-1)}(x)}{(p+1)!} \right\\} h^{p-r+1}\left\\{1 + O(h)\right\\}\right] \xrightarrow{D} N(0,1)$$

If $x$ is near the boundary of the support of $f$ then the above result is slightly changes (see Fan, Heckman and Wand (1995), theorem 1a).

## How does the proof work?

### Standard GLM stuff

Let $\eta(x)$ be the underlying linear predictor (a misnomer in this case). Then The mean ($E\left[Y|X=x\right]$) is $\mu(x)$, and is related to $\eta$ by the link function $g(\cdot)$:

$$\eta(x) = g(\mu(x))$$

or, equivalently:

$$\mu(x) = g^{-1}(\eta(x))$$.

Note that in this case, the $x$ variable is the location.

### Estimating a nonparametric $\eta$
We're interested in estimating the $eta$ function at each location. So do a Taylor expansion of $\eta$ around our fixed location $x$:

$$\eta(y) \approx \eta(x) + \nabla \eta(x)(y-x) + O((y-x)^2)$$

and since $y-x \< h\_n$ ($h\_n$ the bandwidth), we have that 

$$\eta(y) \approx \eta(x) + \nabla \eta(x)(y-x) + o(h\_n^2)$$

Let $\eta\_k(x) = \nabla^k \eta(x)$. Then we are estimating $\eta(x)$ by $\hat{\eta}\_0$.

### Broad strokes
The main theorem is concerned with the asymptotic distribution of $\hat{\beta}^\*$. Its result is proven from lemmas 1 and 2, and then Theorem 1 follows as the marginal result (read off element-by-element) for $\hat{\beta}^\*\_0$.


#### Quadratic approximation lemma (Fan, Heckman, and Wand, 1995)
This lemma states that if $c\_n(\theta) is a sequence of convex functions of the form

$$c\_n(\theta) = U\_n^T \theta - 1/2 \theta^T(\boldsymbol{F} + \alpha\_n \boldsymbol{G}) \theta +f\_n(\theta)$$

then under some conditions, $\hat{\theta}\_n = \boldsymbol{F}^{-1}\boldsymbol{U}\_n + o\_p(1)$ minimizes $c\_n$ and if $f'(\theta) = o\_p(\alpha\_n)$ uniformly in a neighborhood of $\hat{\theta}$, then

$$ \hat{\theta}\_n = \boldsymbol{F}^{-1}\boldsymbol{U}\_n - \alpha\_n \boldsymbol{F}^{-1} \boldsymbol{G} \boldsymbol{F}^{-1} \boldsymbol{U}\_n + o\_p(\alpha\_n).$$


#### Lemma 1
Lemma 1 is concerned with showing that the estimates $\hat{\beta}^*$ satisfy the conditions of the quadradtic approximation lemma.

#### Lemma 2
This lemma is concerned with the asymptotic variance of the $\hat{\beta}$ estimates. 

### Greater detail
At location $x$, ask what would be my estimate of $\eta$, based on this nearby observation $X\_i$? 

That estimate comes from "linearizing" the function $\eta(\cdot)$ near $x$ via he Taylor expansion.

Based on obsered data $(X\_i, Y\_i)$, the function $\eta$ should be like

$$Y\_i \approx g^{-1}(\eta(X\_i))$$

or

$$\eta(X\_i) \approx g(Y\_i)$$

so

$$g(Y\_i) \approx \eta(X\_i) = \eta(x) + \nabla \eta(x)(X\_i-x) + o(h)$$

And the $\eta(x)$ part is like an intercept, while the $\nabla \eta(x)$ part is like a slope for $(X\_i - x)$. So in matrix form,

$$\begin{bmatrix}g(Y\_1) \\\\ g(Y\_2) \\\\ g(Y\_3) \\\\ \dots \\\\ g(Y\_n)\end{bmatrix} = \begin{bmatrix}1 & (X\_1 - x) \\\\ 1 & (X\_2 - x) \\\\ 1 & (X\_3 - x) \\\\ \dots & \dots \\\\ 1 & (X\_n - x)\end{bmatrix}^T \times \begin{bmatrix} \eta(x) \\\\ \nabla \eta(x) \end{bmatrix}$$

But not each observation is treated equally. We weight the observations based on their distance from $x$, using the kernel function $K\_h(s)$