# Non-Separable Data

*Can linear SVM solve the following classification problem?*

<Image src='../sepa-2.png' class='w-80 mx-auto'/>

---

# Non-Separable Data

The basic assumption of hard margin SVM is too rigorous.
- All the samples should lie outside the margin and be correctly classified, i.e., $y_i (\bm{w}'\bm{x}_i + b) \geq 1$.

This assumption can be relaxed as follows:
- Allow some samples to be classified incorrectly, i.e., $y_i (\bm{w}'\bm{x}_i + b) < 1$.

<Image src='../nonsepa-1.png' class='w-40 mx-auto'/>

---

# Soft Margin

The mathematical model of SVM can be changed as follows:
- Remove the old constraint $y_i (\bm{w}'\bm{x}_i + b) \geq 1$ to allow incorrect classifications.
- Add penalty $\ell \left( y_i (\bm{w}'\bm{x}_i + b) \right)$ to the objective function.

*We've turned the hard margin into a **soft margin**!* 
$$
\begin{aligned}
& \min_{\bm{w}, b} \frac{\left\|\bm{w}\right\|^2}{2} \\
\text{s.t.}\quad& y_i (\bm{w}'\bm{x}_i + b) \geq 1, \quad i=1,2,\dots,n \\
&\qquad\Downarrow \\
\min_{\bm{w},b} \frac{\left\|\bm{w}\right\|^2}{2} & 
+ \textcolor{crimson}{C} 
\textcolor{royalblue}{\sum_{i=1}^{n} \ell \left( y_i (\bm{w}'\bm{x}_i + b) \right)}
\end{aligned}
$$

<v-click at=1>

<div class="fixed left-15 bottom-29">
<p v-mark="{ at: 1, color: 'crimson' }" class="italic font-serif text-crimson text-lg" >
regularization coefficient (positive)
</p>
</div>
<Arrow x1="400" y1="450" x2="333" y2="410" color='crimson' width="1" arrowSize="1" />

<div class="fixed left-160 bottom-26 text-royalblue">
<p v-mark.royalblue=1 class="italic font-serif text-royalblue text-lg" >
penalty for wrong classifications
</p>
</div>
<Arrow x1="550" y1="450" x2="620" y2="420" color='royalblue' width="1" arrowSize="1" />

</v-click>

---

# Soft Margin

[How to define the penalty?]{.subtitle}

<v-clicks>

- Cross entropy loss (like logistic regression)
  - Not suitable because SVM doesn't provide a probabilistic output.
- $0-1$ loss
  - Encodes a hard $0$ and $1$ for incorrect and correct classification respectively.
    $$
    \begin{aligned}
    \ell_{0-1}(\textcolor{royalblue}{z}) = \begin{cases}
    0, & \text{if } z \geq 0 \\ 1, & \text{if } z < 0
    \end{cases}
    \end{aligned}
    $$
  - No penalty for correct classifications
  - Penalty of $1$ for incorrect classifications

</v-clicks>

<v-click at=2>

<div class="fixed left-47 bottom-32 text-royalblue">
<p v-mark.royalblue=2 class="italic font-serif text-royalblue text-lg" >

$z = y_i (\bm{w}'\bm{x}_i + b)$
</p>
</div>
<Arrow x1="448" y1="370" x2="355" y2="390" color='royalblue' width="1" arrowSize="1" />

</v-click>

---
layout: two-cols-header
---

# Soft Margin

[How to define the penalty?]{.subtitle}

::left::

<div class='h-20 inline-block'/>
$$
\begin{aligned}
\ell_{0-1}(z) = \begin{cases}
0, & \text{if } z \geq 0 \\ 1, & \text{if } z < 0
\end{cases}  
\end{aligned}
$$

::right::

<Image src='../loss-zo.png' class='w-80 mx-auto mt-10' />

::bottom::

<div class='-mt-30'>

- ==$0-1$ loss== is **non-convex**.
- All the incorrect classifications are penalized with $1$.

</div>

---
layout: two-cols-header
---

# Soft Margin

[How to define the penalty?]{.subtitle}

::left::

<div class='h-25 inline-block'/>

$$
\ell_{h}(z) = \max(0, 1-z)
$$

::right::

<Image src='../loss-hinge.png' class='w-96 mx-auto mt-3' />

::bottom::

<div class='-mt-25'>

- ==Hinge loss== is **convex**.
- Samples further from the margin boundary have large penalty.

</div>

---

# Soft Margin SVM

The optimization problem is formulated as follows:
$$
\min_{\bm{w},b} \frac{\left\|\bm{w}\right\|^2}{2} + C \sum_{i=1}^{n} \max \left(0, 1 - y_i (\bm{w}'\bm{x}_i + b) \right)
$$

$$
\text{Linear SVM} + \text{Hinge loss} = \text{Soft margin SVM}
$$

::alert
How to address the non-differentiable objective function?
::

<v-click>

- Use sub-gradient descent
- Transform the problem to a differentiable one

</v-click>

---
layout: two-cols-header
---

# Soft Margin SVM

Let's transform the problem by introducing the slack variable $\xi_i$: 
$$
\begin{aligned}
    &\min_{\bm{w},b,\bm{\xi}} \frac{\left\|\bm{w}\right\|^2}{2}
    + C \sum_{i=1}^{n} \textcolor{crimson}{\xi_i}\\
    \text{s.t.}\quad& y_i (\bm{w}'\bm{x}_i + b) \geq 1 - \xi_i, \quad i=1,2,\dots,n \\
    &\xi_i \geq 0, \quad i=1,2,\dots,n 
\end{aligned}
$$

<div class="fixed left-150 top-33 text-crimson">
<p v-mark="{ at: 0, color: crimson }" class="italic font-serif text-crimson text-lg" >

$\xi_i = \max(0, 1 - y_i (\bm{w}'\bm{x}_i + b))$

</p>
</div>
<Arrow x1="550" y1="175" x2="580" y2="165" color=crimson width="1" arrowSize="1" />

::left::

<Image src='../hinge-xi.png' class='w-65 mx-auto mt-45'/>

::right::

<div class='h-52 inline-block'/>

$$
\begin{aligned}
\xi_i & \geq 1-z_i \\ 
\xi_i & \geq 0
\end{aligned}
$$

---

# Dual Problem of Soft Margin SVM

Again, we need to use Lagrangian dual to deal with complex constraints.

**Primal constraints**:
1. $1 - \xi_i - y_i (\bm{w}'\bm{x}_i + b) \leq 0$
2. $-\xi_i \leq 0$

**Dual Variables**: $\alpha_i$ for constraint 1, $\mu_i$ for constraint 2.

**Lagrangian**: 
$$
\begin{aligned}
    L(\bm{w},b,\bm{\xi},\bm{\alpha},\bm{\mu}) = &\frac{\left\|\bm{w}\right\|^2}{2}
    + C \sum_{i=1}^{n} \xi_i
    + \sum_{i=1}^{n} \alpha_i \left( 1-\xi_i - y_i (\bm{w}'\bm{x}_i + b) \right) -\sum_{i=1}^{n} \mu_i\xi_i
\end{aligned}
$$

---

# Dual Problem of Soft Margin SVM

*Gradient of Lagrangian*:
$$
\begin{aligned}
    \nabla_{\bm{w}} L(\bm{w}^*, b^*, \bm{\xi}^*, \bm{\alpha}^*, \bm{\mu}^*) =& 0 
    \qquad\Leftrightarrow\qquad \bm{w}^* = \sum\nolimits_{i=1}^n \alpha^*_i y_i \bm{x}_i
    \\
    \nabla_{b} L(\bm{w}^*, b^*, \bm{\xi}^*, \bm{\alpha}^*, \bm{\mu}^*) =& 0 
    \qquad\Leftrightarrow\qquad 0 = \sum\nolimits_{i=1}^n \alpha^*_i y_i
    \\
    \nabla_{\bm{\xi}} L(\bm{w}^*, b^*, \bm{\xi}^*, \bm{\alpha}^*, \bm{\mu}^*) =& 0 
    \qquad\Leftrightarrow\qquad \mu^*_i = C - \alpha^*_i
\end{aligned}
$$

**Dual problem**: 
$$
\begin{aligned}
    \max_{\bm{\alpha}} & \sum\nolimits_{i=1}^n \alpha_i -
    \frac{1}{2} \sum\nolimits_{i=1}^n \sum\nolimits_{j=1}^n \alpha_i \alpha_j y_i y_j \bm{x}'_i \bm{x}_j \\
    \text{s.t.}\quad& \sum\nolimits_{i=1}^n \alpha_i y_i = 0,\\
    & 0 \le \alpha_i \le C, \quad i=1,2,\dots,n  
\end{aligned}
$$

---

# Dual Problem of Soft Margin SVM

**KKT conditions**:

1. *Primal constraints*: $1 - \xi^*_i - y_i ({\bm{w}^*}'\bm{x}_i + b^*) \le 0, \;\xi^*_i \ge 0,\quad i=1,2,\dots,n$
2. *Dual constraints*: $\alpha^*_i \ge 0,\;\mu^*_i \ge 0,\quad i=1,2,\dots,n$
3. *Complementary slackness*: 
$$
\begin{aligned}
\alpha^*_i \left( 1 - \xi^*_i - y_i ({\bm{w}^*}'\bm{x}_i + b^*) \right) =0,\quad i=1,2,\dots,n \\
\mu^*_i \xi^*_i =0,\quad i=1,2,\dots,n        
\end{aligned}
$$
4. *Gradient of Lagrangian*:
<div class='-mt-15'>
$$
\begin{aligned}
    \bm{w}^* &= \sum\nolimits_{i=1}^n \alpha^*_i y_i \bm{x}_i \\
    0 &= \sum\nolimits_{i=1}^n \alpha^*_i y_i \\
    \mu^*_i &= C - \alpha^*_i
\end{aligned}
$$
</div>


---
layout: two-cols-header
---

# Dual Problem of Soft Margin SVM 


::left::

<Image src='../soft-all.png' class='w-90 mx-auto'/>

::right::

- $\alpha^*_i=0,\xi^*_i=0,z^*_i \ge 1$\
    Not a support vector.\
    Hinge loss $= 0$.
- $\alpha^*_i\in(0,C),\xi^*_i=0,z^*_i = 1$\
    Is a support vector.\
    Hinge loss $= 0$.
- $\alpha^*_i=C,\xi^*_i \ge 0,z^*_i \le 1$\
    Is a support vector.\
    Hinge loss $\ge 0$.

<!--
- Support vectors are those causing hinge loss to be greater than zero or precisely on the margin boundary. 

- And the model parameters w & b are determined only by support vectors.
-->
