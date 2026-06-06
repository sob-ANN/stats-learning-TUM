---
layout: two-cols-header
---

# Linearly separable data
[Preliminaries]{.subtitle}

*How will a decision boundary of a linear classifier look like?*

::left::

<Image src="../intro-1.png" figureClass="w-65 mx-auto mt-18"/>

::right::

<div v-click>
<Image src="../intro-2.png" figureClass="w-65 mx-auto mt-18"/>
</div>

---
layout: two-cols-header
---

# Linearly separable data
[Preliminaries]{.subtitle}

*What about these datasets?*

::left::

<Image src="../sepa-1.png" figureClass="w-65 mx-auto mt-18"/>

::right::

<Image src="../sepa-3.png" figureClass="w-65 mx-auto mt-18"/>

---
layout: two-cols-header
---

# Linearly separable data
[Preliminaries]{.subtitle}

*What about these datasets?*

::left::

<Image src="../sepa-2.png" figureClass="w-65 mx-auto mt-18"/>

::right::

<Image src="../sepa-4.png" figureClass="w-65 mx-auto mt-18"/>

---

# Linearly separable data
[Preliminaries]{.subtitle}

::card{title="Linearly Separable"}
It's when two classes of data can be separated from each other by a **hyperplane**.
::

<div class='h-5 inline-block'/>

A ==hyperplane== is a $p-1$-dimensional affine subspace of a $p$-dimensional space.

$$
\bm{w}'\bm{x} + b = w_1 x_1 + w_2 x_2 + \dots + w_p x_p + b = 0
$$

---

# Linearly separable data
[Preliminaries]{.subtitle}

<div class="flex flex-row justify-center mt-6">
  <div class="w-1/3">
    <Image src="../hyperplane-1.png" figureClass="w-60 mx-auto mt-10" alt='1-dim'/>
  </div>
  <div class="w-1/3">
    <Image src="../hyperplane-2.png" figureClass="w-60 mx-auto mt-10" alt='2-dim'/>
  </div>
  <div class="w-1/3">
    <Image src="../hyperplane-3.png" figureClass="w-60 mx-auto mt-10" alt='3-dim'/>
  </div>
</div>

---
layout: two-cols-header
---

# Linearly separable data

==Support vector machine== can make classifications using a separating **hyperplane**. It can perfectly separate linearly separable data. 

::left::

<div class='h-10 inline-block'/>

$$
\begin{gathered}
  \begin{cases}
    y_i = +1, & \text{if}\;\bm{w}'\bm{x}_i + b > 0, \\
    y_i = -1, & \text{if}\;\bm{w}'\bm{x}_i + b < 0
  \end{cases} \\
  \Downarrow\\
  y_i = \text{sgn}(\bm{w}'\bm{x}_i + b)\\
  \Downarrow\\
  y_i (\bm{w}'\bm{x}_i + b) > 0
\end{gathered}
$$

::right::

<Image src='../tri-hyperplane.png' class='w-80 mx-auto mt-2'/>

::bottom::

The central separating hyperplane is the decision boundary.

---
layout: two-cols-header
---

# Margin & Support Vector

::left::

- For linearly separable data, we can always find **a margin** between samples of the two classes.
- ==Support vectors== lie on the the boundary of the margin.
- The separating hyperplane is determined by the support vectors.

::right::

<Image src='../sv.png' class='w-80 mx-auto'/>

<!--
The boundaries can be represented by $wx+b = \pm 1$\
Some might ask why are they 1 and -1. Actually, they can be any number!
-->

---
layout: two-cols-header
---

# Margin & Support Vector

::left::

The distance from an arbitrary point $\bm{x}_0$ to the hyperplane $\bm{w}'\bm{x}+b=0$ can be expressed as:
$$
\mathrm{dist}(\bm{x}_0 \| \bm{w}'\bm{x}+b=0) = \frac{|\bm{w}'\bm{x}_0+b|}{\left\|\bm{w}\right\|}      
$$
We can further obtain the width of margin:
$$
\gamma = \frac{|1|}{\left\|\bm{w}\right\|}
       + \frac{|-1|}{\left\|\bm{w}\right\|}
       = \frac{2}{\left\|\bm{w}\right\|}
$$

::right::

<Image src='../sv.png' class='w-80 mx-auto'/>

---

# Linear SVM

- **Prerequisite**: Linearly separable data.
- **Objective**: Maximize the **[width of margin]{class="text-crimson"}** around separating hyperplane. 
$$
\begin{aligned}
  & \max_{\bm{w}, b} \textcolor{crimson}{\frac{2}{\left\|\bm{w}\right\|}} \\
  \text{s.t.}\quad& y_i (\bm{w}'\bm{x}_i + b) \geq \textcolor{royalblue}{1}, 
  \quad i=1,2,\dots,n
\end{aligned}
$$

::v-click

For computational convenience, the problem can be equivalently transformed into a **convex quadratic program** with linear constraints:
$$
\begin{aligned}
  & \min_{\bm{w}, b} \frac{\left\|\bm{w}\right\|^2}{2} \\
  \text{s.t.}\quad& y_i (\bm{w}'\bm{x}_i + b) \geq 1, \quad i=1,2,\dots,n
\end{aligned}
$$

::

<!--
Note that the constraints is **greater than 1** not 0.\
In our current model with linearly separable data, there is no samples inside the margin.
-->

---

# Dual problem of Linear SVM

*How to deal with inequality constraints in convex optimization?*
$$
\begin{aligned}
  & \min_{\bm{w}, b} \frac{\left\|\bm{w}\right\|^2}{2} \\
  \text{s.t.}\quad& \textcolor{royalblue}{y_i (\bm{w}'\bm{x}_i + b) \geq 1}, \quad i=1,2,\dots,n
\end{aligned}
$$

<v-click at=1>

<Marker color='red'>Lagrange Multiplier Method</Marker>: Transform a constrained problem by augmenting the objective function with a weighted sum of the constraint functions.
**Lagrangian** $L: \mathbb{R}^m \times \mathbb{R} \times \mathbb{R}^n \to \mathbb{R}$:
$$
\begin{aligned}
  L(\bm{w}, b, \bm{\alpha}) = 
  \frac{\left\|\bm{w}\right\|^2}{2} + \sum_{i=1}^n \textcolor{crimson}{\alpha_i} 
  \left( 1 - y_i \left( \bm{w}'\bm{x}_i + b \right) \right)
\end{aligned}
$$

<div class="fixed left-158 top-40 text-royalblue">
<p v-mark.royalblue=1 class='text-lg'>

$1 - y_i (\bm{w}'\bm{x}_i + b) \leq 0$
</p>
</div>
<Arrow x1="520" y1="220" x2="620" y2="190" color='royalblue' width="1" arrowSize="1" />

<div class="fixed left-140 bottom-30">
<p v-mark="{ at: 1, color: 'crimson' }" class="italic font-serif text-crimson text-lg" >
Lagrangian multiplier (non-negative)
</p>
</div>
<Arrow x1="530" y1="433" x2="550" y2="410" color='crimson' width="1" arrowSize="1" />

</v-click>

<!--
**It's hard to directly solve a problem with many inequality constraints.**

1. We have to write all the inequality constraints to a form of **not greater than**.
2. Then we lift the left hand side up to the objective function. And add a coefficient.
3. Since we have lots of constraints, one for each training sample, we need to summation notation here.
-->


---

# Dual problem of Linear SVM

**Primal problem**: 
$$
\begin{aligned}
  & \min_{\bm{w}, b} \frac{\left\|\bm{w}\right\|^2}{2} \\
  \text{s.t.}\quad& 1 - y_i (\bm{w}'\bm{x}_i + b) \leq 0, 
  \quad i=1,2,\dots,n  
\end{aligned}
$$

**Lagrangian**: 
$$
\begin{aligned}
  L(\bm{w}, b, \bm{\alpha}) = 
  \frac{\left\|\bm{w}\right\|^2}{2} + \sum_{i=1}^n \alpha_i
  \left( 1 - y_i \left( \bm{w}'\bm{x}_i + b \right) \right)  
\end{aligned}
$$

**Dual problem**:
<div class='-mt-12'>
$$
\begin{gathered}
  \max_{\textcolor{crimson}{\bm{\alpha} \succeq 0}}
  \min_{\bm{w}, b} L(\bm{w}, b, \bm{\alpha})
\end{gathered}
$$
</div>

<!--
The biggest challenge in the dual problem is that we still have a constraint here. alpha >= 0
-->

---

# Dual problem of Linear SVM

*Gradient of Lagrangian*: 
$$
\begin{aligned}
  \nabla_{\bm{w}} L(\bm{w}^*, b^*, \bm{\alpha}^*) =& 0 
  \qquad\Leftrightarrow\qquad \bm{w}^* = \sum\nolimits_{i=1}^n \alpha^*_i y_i \bm{x}_i
  \\
  \nabla_{b} L(\bm{w}^*, b^*, \bm{\alpha}^*) =& 0 
  \qquad\Leftrightarrow\qquad 0 = \sum\nolimits_{i=1}^n \alpha^*_i y_i  
\end{aligned}
$$

**Dual problem**:
$$
\begin{aligned}
  \max_{\bm{\alpha}} & \sum\nolimits_{i=1}^n \alpha_i -
  \frac{1}{2} \sum\nolimits_{i=1}^n \sum\nolimits_{j=1}^n \alpha_i \alpha_j y_i y_j \bm{x}'_i \bm{x}_j \\
  \text{s.t.}\quad& \sum\nolimits_{i=1}^n \alpha_i y_i = 0,\\
  & \alpha_i \geq 0, \quad i=1,2,\dots,n  
\end{aligned}
$$

<!--
Let's first ignore the constraint of alpha. \
For an unconstrained convex problem, the only condition to ensure our solution is optimal  is by **letting the gradient to be zero**.

By substituting the results back to the dual problem, we can rewrite it in this form. \
Although it looks intimidating, it is actually much simpler to solve than the primal problem. There are many off-the-shelf algorithms to address it. \
Due to time limit, we will not get into the details on these algorithms. If you are interested, you can either take some operations research lectures or search for some related materials.
-->


---

# Dual problem of Linear SVM

**KKT conditions**:

1. *Primal constraints*: <span class='empty-space'/> $1 - y_i ({\bm{w}^*}'\bm{x}_i + b^*) \leq 0,\quad i=1,2,\dots,n$
2. *Dual constraints*: <span class='empty-space'/> $\alpha^*_i \geq 0,\quad i=1,2,\dots,n$
3. *Complementary slackness*:
$$
\alpha^*_i \left( 1 - y_i ({\bm{w}^*}'\bm{x}_i + b^*) \right) =0,\quad i=1,2,\dots,n
$$
4. *Gradient of Lagrangian*: 
$$
\begin{aligned}
  \nabla_{\bm{w}} L(\bm{w}^*, b^*, \bm{\alpha}^*) =& 0 \\
  \nabla_{b} L(\bm{w}^*, b^*, \bm{\alpha}^*) =& 0
\end{aligned}
$$

<!--
The first two conditions are quite simple:
- Primal constraints are those in the original problem. 
- Dual constraints are those in the dual problem. 

Complementary slackness is the actually taken from the Lagrangian function. 

Finally, we have to ensure the zero gradient, which is also quite basic.
-->

---
layout: two-cols-header
---

# Dual problem of Linear SVM

::left::

According to ++(3) complementary slackness++, we have
either
- $y_i({\bm{w}^*}'\bm{x}_i + b^*) = 1$. This indicates that the $i$-th sample is a **support vector**.
- or $\alpha^*_i = 0$. This indicates the $i$-th sample is outside the margin.

::right::

<Image src='../sv.png' class='w-80 mx-auto'/>

---

# Dual problem of Linear SVM

Assume the optimal solution of the dual problem is $\bm{\alpha}^*$. Using ++(4) gradient of Lagrangian++, we've already obtained:
$$
\bm{w}^* = \sum_{i=1}^n \textcolor{crimson}{\alpha^*_i} y_i \bm{x}_i
$$

Denote an arbitrary support vector as $\bm{x}_s, y_s$. Using ++(3) complementary slackness++, we can obtain: 
$$
b^* = \textcolor{mediumseagreen}{\frac{1}{y_s}} - \sum_{i=1}^n \alpha^*_i y_i \bm{x}'_i \bm{x}_s = {y_s} - \sum_{i=1}^n \alpha^*_i y_i \bm{x}'_i \bm{x}_s  
$$

> $\bm{w}^*$ and $b^*$ are only determined by support vectors.

