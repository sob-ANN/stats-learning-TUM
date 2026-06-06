---
layout: two-cols-header
---

# Non-Separable Case 

Problems that have been solved by hard/soft-margin linear SVM.

::left::

<Image src='../intro-2.png' figureClass='w-60 mx-auto mt-10' alt='Linearly Separable'/>

::right::

<Image src='../nonsepa-1.png' figureClass='w-60 mx-auto mt-10' alt='Nearly Linearly Separable'/>

---
layout: two-cols-header
---

# Non-Separable Case 

*What about these cases?*

::left::

<Image src='../sepa-3.png' figureClass='w-60 mx-auto mt-10'/>

::right::

<Image src='../sepa-4.png' figureClass='w-60 mx-auto mt-10'/>

---

# Distance Measure

*How could space travel be realized?*

<Image src='../falcon.gif' Class='w-120 mx-auto mt-10' />

---

# Distance Measure

*Is distance on map the same as actual travel distance?*

<Image src='../munichnet.png' figureClass='w-122 mx-auto' alt='Munich Rail Network (Source: MVV-München)'/>

---
layout: two-cols-header
---

# Distance Measure

By distorting the space, we are actually redefining the distance in a space.

In a two-dimensional Euclidean space, the distance between two points is defined as:
$$
d(\bm{x}, \bm{y}) = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2}
$$

However, for a generic mathematical space, the definition of distance is not necessarily Euclidean.

::left::

<div class='h-45 inline-block'/>

$$
d(\bm{x}, \bm{y}) = f(\bm{x}, \bm{y})
$$

::alert
Can we define a new distance measure for the data on the right?
::

::right::

<Image src='../map-1.png' figureClass='w-50 mx-auto mt-45'/>

---
layout: two-cols-header
---

# Feature Space Mapping

Build a mapping $\phi(\bm{x})$ to map features in the original feature space to a new one such that samples of two classes become linearly separable. 
$$
\phi(\bm{x}) = \bm{x}^2
$$

::left::

<Image src='../map-1.png' figureClass='w-60 mx-auto mt-28'/>

::right::

<Image src='../map-2.png' figureClass='w-60 mx-auto mt-28'/>

---
layout: two-cols-header
---

# Feature Space Mapping

We can also map the features to a higher dimensional space.\
The separating hyperplane in the new space will be: 
$$
\bm{w}'\phi(\bm{x}) + b = 0
$$

::left::

<Image src='../map-1.png' figureClass='w-60 mx-auto mt-28'/>

::right::

<Image src='../map-3.png' figureClass='w-60 mx-auto mt-28'/>

---

# Nonlinear SVM

<div class='h-10 inline-block'/>

Replace the expression of hyperplane in soft-margin linear SVM by the new transformed hyperplane yields: 
$$
\begin{aligned}
    &\min_{\bm{w},b,\bm{\xi}} \frac{\left\|\bm{w}\right\|^2}{2}
    + C \sum_{i=1}^{n} \xi_i \\
    \text{s.t.}\quad& y_i \left( \textcolor{royalblue}{\bm{w}'\phi(\bm{x}_i) + b} \right) 
    \geq 1 - \xi_i, \quad i=1,2,\dots,n \\
    &\xi_i \geq 0, \quad i=1,2,\dots,n 
\end{aligned}
$$

---

# Dual Problem of Nonlinear SVM

Following the same derivation as linear SVM, we can obtain the dual problem of nonlinear SVM: 
$$
\begin{aligned}
    \max_{\bm{\alpha}} & \sum_{i=1}^n \alpha_i -
    \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y_i y_j 
    \textcolor{crimson}{\phi(\bm{x}_i)' \phi(\bm{x}_j)} \\[20pt]
    \text{s.t.}\quad& \sum_{i=1}^n \alpha_i y_i = 0,\\
    & 0 \le \alpha_i \le C, \quad i=1,2,\dots,n
\end{aligned}
$$

The only difference is that we apply an mapping on $\bm{x}_i$ and $\bm{x}_j$ before computing their inner product.

<div class="fixed right-20 top-65 text-crimson">
<p v-mark="{ at: 0, color: crimson }" class="italic font-serif text-crimson text-lg" >

$\phi(\bm{x}_i)' \phi(\bm{x}_j) = \langle \phi(\bm{x}_i), \phi(\bm{x}_j) \rangle$
</p>
</div>
<Arrow x1="666" y1="240" x2="666" y2="270" color='crimson' width="1" arrowSize="1" />


---
layout: two-cols-header
---

# Kernel Trick

::left::

The issue of the mapping $\phi(\bm{x})$:

- It's hard to find an appropriate $\phi(\bm{x})$ for the data.
- Computational cost can be huge when dealing with high-dimensional features.

::right::

<Image src='../ball-sepa.png' figureClass='w-90 mx-auto'/>

---

# Kernel Trick

Let's get back the dual problem of nonlinear SVM:
$$
\begin{aligned}
    \max_{\bm{\alpha}} & \sum_{i=1}^n \alpha_i -
    \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y_i y_j 
    \textcolor{crimson}{\phi(\bm{x}_i)' \phi(\bm{x}_j)} \\
    \text{s.t.}\quad& \sum_{i=1}^n \alpha_i y_i = 0,\\
    & 0 \le \alpha_i \le C, \quad i=1,2,\dots,n
\end{aligned}
$$

::alert
Do we really need to know $\phi(\bm{x})$ explicitly?
::

<!-- 
Every time when we deal with features, we have to compute the inner product of two features.
-->

---

# Kernel Trick

Use a surrogate function $\kappa(\bm{x}_i, \bm{x}_j) = \phi(\bm{x}_i)' \phi(\bm{x}_j)$.

The separating hyperplane will be transformed to: 
$$
\begin{aligned}
    \bm{w}'\phi(\bm{x}) + b = 0 
    \Rightarrow &\left( \sum_{i=1}^n \alpha^*_i y_i \phi(\bm{x}_i) \right)'\phi(\bm{x}) + b = 0 \\
    \Rightarrow &\sum_{i=1}^n \alpha^*_i y_i \kappa(\bm{x}_i, \bm{x}) + b = 0   
\end{aligned}
$$

==Kernel trick== helps us to ++skip the procedure of *mapping* and *computing inner product*++ by directly defining the inner product in the new space.

---

# Kernel Trick

<div class='h-8 inline-block'/>

::card{title='Kernel Function'}
If there exists a mapping $\phi: \mathcal{X} \to \mathcal{H}$, for any $\bm{x}_1, \bm{x}_2 \in\mathcal{X}$, the kernel function $\kappa: \mathcal{X}\times \mathcal{X}\to \mathbb{R}$ is given by:
$$
\kappa(\bm{x}_1, \bm{x}_2) = \phi(\bm{x}_1)' \phi(\bm{x}_2) = \langle \phi(\bm{x}_1), \phi(\bm{x}_2) \rangle_{\mathcal{H}}
$$
::

> The kernel function redefines the **similarity** rather than **distance** between samples.

---

# Common Kernel Functions

Commonly used kernel functions include:
- Linear kernel
- Radial basis function (RBF) kernel (aka Gaussian kernel)
- Polynomial kernel
- Matérn kernel
- Sigmoid kernel (aka hyperbolic tangent kernel)

---

# Common Kernel Functions

==Linear kernel==:
No transformation is performed. 
$$
\kappa_\text{linear}(\bm{x}_1, \bm{x}_2) = \bm{x}'_1 \bm{x}_2
$$

==Polynomial kernel==:
$$
\kappa_{\text{poly},d}(\bm{x}_1, \bm{x}_2) = (\bm{x}'_1 \bm{x}_2 + c)^d    
$$ 
where $d \in \mathbb{Z}_{+}, c \in \mathbb{R}$ are the hyperparameters of polynomial kernel.

---

# Common Kernel Functions

==RBF kernel==:
RBF kernel is the most widely used kernel. 
$$
\kappa_\text{RBF}(\bm{x}_1, \bm{x}_2) = \exp \left( - \frac{(\bm{x}_1 - \bm{x}_2)' (\bm{x}_1 - \bm{x}_2)}{2\sigma^2} \right)
$$
where $\sigma>0$ is the hyperparameter of RBF kernel.


==Anisotropic RBF kernel==:
$$
\kappa_{\text{poly},d}(\bm{x}_1, \bm{x}_2) = \exp \left( - \frac{1}{2} (\bm{x}_1 - \bm{x}_2)' \Sigma (\bm{x}_1 - \bm{x}_2) \right)  
$$
where $\Sigma = \text{diag}(\sigma_1^2, \sigma_2^2, \dots, \sigma_m^2)$ is the hyperparameter of anisotropic RBF kernel that defines a different $\sigma$ for each dimension of feature.

---

# Common Kernel Functions

==Rational quadratic kernel==:
$$
\kappa_\text{rq}(\bm{x}_1, \bm{x}_2) = \left( 1 + \frac{(\bm{x}_1 - \bm{x}_2)' (\bm{x}_1 - \bm{x}_2)}{2 \alpha l^2} \right)^{-\alpha}
$$
where $\alpha>0, l>0$ are the hyperparameters of rational quadratic kernel.


==Sigmoid kernel==:
Sigmoid kernel comes from neural network.
$$
\kappa_\text{sigmoid}(\bm{x}_1, \bm{x}_2) = \tanh(\alpha \bm{x}'_1 \bm{x}_2 + \beta)
$$
where $\alpha>0,\beta<0$ are the hyperparameters of sigmoid kernel.

---

# Common Kernel Functions

<Image src='../kernel-mat.png' figureClass='w-160 mx-auto'/>

---

# Common Kernel Functions

**Note**: Not all functions are eligible as kernel functions (i.e., a valid mapping $\phi$ exists).

==Mercer's condition==:
Define a Gram matrix (aka kernel matrix) of a **symmetric** function $\kappa$ with respect to **any** $\bm{x}_1, \bm{x}_2, \dots, \bm{x}_n$: 
$$
\begin{aligned}
      \bm{K} = \begin{pmatrix}
        \kappa(\bm{x}_1, \bm{x}_1) & \kappa(\bm{x}_1, \bm{x}_2) & \cdots & \kappa(\bm{x}_1, \bm{x}_n) \\
        \kappa(\bm{x}_2, \bm{x}_1) & \kappa(\bm{x}_2, \bm{x}_2) & \cdots & \kappa(\bm{x}_2, \bm{x}_n) \\
        \vdots & \vdots & \ddots & \vdots \\
        \kappa(\bm{x}_n, \bm{x}_1) & \kappa(\bm{x}_n, \bm{x}_2) & \cdots & \kappa(\bm{x}_n, \bm{x}_n)
      \end{pmatrix}    
\end{aligned}
$$

A ++semi-positive definite++ Gram matrix ensures this function to be a valid kernel function.

> *Exceptions*: Some kernel functions like sigmoid kernel do not always satisfy Mercer's condition.


---

# Common Kernel Functions

<div class='h-5 inline-block'/>

::card{title='Composition of kernels'}
Compositions of Mercer kernel functions still satisfy Mercer's condition.
$$
\begin{aligned}
      \kappa(\bm{x}_1, \bm{x}_2) = \begin{cases}
        c\kappa_1(\bm{x}_1, \bm{x}_2) \\
        \kappa_1(\bm{x}_1, \bm{x}_2) + \kappa_2(\bm{x}_1, \bm{x}_2) \\
        \kappa_1(\bm{x}_1, \bm{x}_2) \cdot \kappa_2(\bm{x}_1, \bm{x}_2) 
      \end{cases}    
\end{aligned}
$$
::
