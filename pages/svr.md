---
layout: two-cols-header
---

# Support Vector Regression

Although SVM was originally designed for solving classification problems, it can also be modified for regression problems.

::left::

<div class='h-10 inline-block'/>

**Basic idea**: Create a $\epsilon$-tube around $f(\bm{x})$, within which the samples are assumed to be correctly predicted.

- Ideally, the goal is to find a function that deviates from the actual target values by a value no greater than $\epsilon$.
- Samples outside this $\epsilon$-tube incur a loss.

::right::

<Image src='../svr-margin.png' figureClass='w-96 mx-auto mt-12'/>

---
layout: two-cols-header
---

# Support Vector Regression

The generic definition of SVR is:
$$
\begin{aligned}
    \min_{\bm{w},b} \frac{\|w\|^2}{2} + 
    \textcolor{crimson}{C}
    \sum_{i=1}^N \textcolor{royalblue}{\ell_{\epsilon} \left( f(\bm{x}_i) - y_i \right)}
\end{aligned}
$$

::left::

<div class='h-33 inline-block'/>
$$
\begin{aligned}
\ell_{\epsilon}(\textcolor{coral}{z}) = \begin{cases}
    0, & \text{if } |z| \le \epsilon; \\
    |z| - \epsilon, & \text{otherwise}
\end{cases}
\end{aligned}
$$

::right::

<Image src='../epi-loss.png' figureClass='w-70 mx-auto mt-30'/>

::bottom::

::alert
Is there any connection with ridge regression?
::

<div class="fixed left-33 top-55 text-crimson">
<p v-mark="{ at: 0, color: crimson }" class="italic font-serif text-crimson text-lg" >
regularization coefficient (positive)
</p>
</div>
<Arrow x1="460" y1="210" x2="400" y2="250" color='crimson' width="1" arrowSize="1" />

<div class="fixed right-30 top-40 text-royalblue">
<p v-mark="{ at: 0, color: royalblue }" class="italic font-serif text-royalblue text-lg" >

$\epsilon$-insensitive loss
</p>
</div>
<Arrow x1="690" y1="190" x2="720" y2="190" color='royalblue' width="1" arrowSize="1" />

<div class="fixed left-15 bottom-30 text-coral">
<p v-mark="{ at: 0, color: coral }" class="italic font-serif text-coral text-lg" >

$z_i = f(\bm{x}_i) - y_i$
</p>
</div>
<Arrow x1="150" y1="343" x2="150" y2="380" color='coral' width="1" arrowSize="1" />

<!--
- Error + w penalty
- It can also be solved by sub-gradient algorithm or introducing slack variable
-->

---

# Support Vector Regression

Similar to what we did to soft-margin linear SVM, the problem can be transformed by introducing slack variables $\xi^{+}_i, \xi^{-}_i$.
$$
\begin{aligned}
    \min_{\bm{w},b,\bm{\xi^{+}},\bm{\xi^{-}}} 
    &\frac{\|w\|^2}{2} + C \sum\nolimits_{i=1}^N (\xi^{+}_i + \xi^{-}_i) \\
    \text{s.t.}\quad 
    & f(\bm{x}_i) - y_i \le \epsilon + \xi^{+}_i, \quad i=1,2,\dots,n\\
    & y_i - f(\bm{x}_i) \le \epsilon + \xi^{-}_i, \quad i=1,2,\dots,n\\
    & \xi^{+}_i, \xi^{-}_i \ge 0, \quad i=1,2,\dots,n
\end{aligned}
$$

*Hints*: Rewrite the $\epsilon$-insensitive loss as the sum of two sub-functions:
$$
\ell_{\epsilon}(z) = \ell_{\epsilon}^+(z) + \ell_{\epsilon}^-(z)
\qquad\qquad
\begin{matrix}
  \ell_{\epsilon}^+(z) = \max(0, z - \epsilon) \\
  \ell_{\epsilon}^-(z) = \max(0, -z - \epsilon) 
\end{matrix}
$$

---

# Dual Problem of SVR

**Primal constraints**:
<div class='flex flex-row justify-between'>
<div class='w-1/2'>

1. $f(\bm{x}_i) - y_i \le \epsilon + \xi^{+}_i$
2. $y_i - f(\bm{x}_i) \le \epsilon + \xi^{-}_i$
</div>
<div class='w-1/2'>

3. $\xi^{+}_i \ge 0$
4. $\xi^{-}_i \ge 0$
</div>
</div>

**Dual Variables**: $\alpha^+_i, \alpha^-_i, \mu^+_i, \mu^-_i$ for constraints 1--4.

**Lagrangian**:
<div class='-mt-12'>
$$
\begin{aligned}
    & L(\bm{w},b,\bm{\xi}^+,\bm{\xi}^-,\bm{\alpha}^+,\bm{\alpha}^-,\bm{\mu}^+,\bm{\mu}^-)\\
    =& \frac{\left\|\bm{w}\right\|^2}{2}
    + C \sum\nolimits_{i=1}^{n} (\xi^+_i + \xi^-_i)
    - \sum\nolimits_{i=1}^{n} \mu^+_i\xi^+_i - \sum\nolimits_{i=1}^{n} \mu^-_i\xi^-_i \\
    & +\sum\nolimits_{i=1}^{n} \alpha^+_i \left( f(\bm{x}_i) - y_i - \epsilon - \xi^+_i \right) \\
    & +\sum\nolimits_{i=1}^{n} \alpha^-_i \left( y_i - f(\bm{x}_i) - \epsilon - \xi^-_i \right)  
\end{aligned}
$$
</div>

---

# Dual Problem of SVR 

*Gradient of Lagrangian*: 
$$
\begin{aligned}
    \nabla_{\bm{w}} L =& 0 
    \;\Leftrightarrow\; \bm{w}^* = \sum\nolimits_{i=1}^n (\alpha^{-*}_i - \alpha^{+*}_i) \phi(\bm{x}_i)
    \\
    \nabla_{b} L =& 0 
    \;\Leftrightarrow\; 0 = \sum\nolimits_{i=1}^n (\alpha^{-*}_i - \alpha^{+*}_i)
    \\
    \nabla_{\bm{\xi}^+} L =& 0 
    \;\Leftrightarrow\; \mu^{+*}_i = C - \alpha^{+*}_i
    \\
    \nabla_{\bm{\xi}^-} L =& 0 
    \;\Leftrightarrow\; \mu^{-*}_i = C - \alpha^{-*}_i
\end{aligned}
$$

The prediction for new data points can be expressed as:
$$
\begin{aligned}
    f(\bm{x}) &= {\bm{w}^*}' \textcolor{royalblue}{\phi(\bm{x})} + b 
    = \left( \sum\nolimits_{i=1}^n (\alpha^{-*}_i - \alpha^{+*}_i) \phi(\bm{x}_i) \right)'
    \phi(\bm{x}) + b \\
    &= \sum\nolimits_{i=1}^n (\alpha^{-*}_i - \alpha^{+*}_i) \textcolor{royalblue}{\kappa(\bm{x}, \bm{x}_i)} + b
\end{aligned}
$$

---

# Dual Problem of SVR

Substitute into Lagrangian to get the dual problem of SVR.

**Dual problem**: 
$$
\begin{aligned}
    \max_{\bm{\alpha}^+,\bm{\alpha}^-} & 
    \sum_{i=1}^n (\alpha^-_i - \alpha^+_i) y_i - 
    \epsilon \sum_{i=1}^n (\alpha^-_i + \alpha^+_i)\\
    & - \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n 
    (\alpha^-_i - \alpha^+_i) (\alpha^-_j - \alpha^+_j) \kappa(\bm{x}_i,\bm{x}_j)\\
    \text{s.t.}\quad& \sum_{i=1}^n (\alpha^-_i - \alpha^+_i) = 0,\\
    & 0 \le \alpha^+_i, \alpha^-_i \le C, \quad i=1,2,\dots,n
\end{aligned}
$$


---
layout: two-cols-header
---

# Dual Problem of SVR

::left::

<Image src='../svr-all.png' figureClass='w-80 mx-auto mt-10'/>

::right::

- $\alpha^{+*}_i > 0, \alpha^{-*}_i = 0$\
  Is a support vector.
- $\alpha^{+*}_i = 0, \alpha^{-*}_i > 0$\
  Is a support vector.
- $\alpha^{+*}_i = \alpha^{-*}_i = 0$\
  Not a support vector.

Prediction is the weighted sum of support vectors. 
$$
f(\bm{x}) = \sum\nolimits_{i=1}^n (\alpha^{-*}_i - \alpha^{+*}_i) \kappa(\bm{x}, \bm{x}_i) + b
$$
