# Hyperparameter

In addition to model parameters, statistical learning models often come with many hyperparameters, which cannot be optimized during the learning process.

<div class="flex flex-row gap-8 w-full">

<div>

- **Model structure**
  - Maximum depth of decision tree
  - Number of trees in random forest
  - Number of layers of neural network
- **Optimization**
  - Learning rate
  - Batch size

</div>
<div>

- **Regularization**
  - $L_2$ penalty
  - $C$ of SVM
- **Preprocessing**
  - Data scaling
- $\ldots$

</div>

</div>

::alert
Why can't we directly optimize hyperparameters?
::

---

# Grid Search & Random Search

- ==Grid search== exhaustively evaluates all possible hyperparameter combinations to find the best-performing model configuration.
- ==Random search== evaluates a random subset of hyperparameter combinations to efficiently explore the search space.

<Image src="../hpo-grid.png" figureClass="w-full mt-5" class="h-50 mx-auto" alt="Source: Bergstra & Bengio (2012) Random Search for Hyper-Parameter Optimization" />

---

# Swarm and Evolutionary Computation

- ==Swarm intelligence== algorithms works by mimicking the collective behavior of natural systems like bird flocks or fish schools.
  - Particle swarm optimization, artificial bee colony algorithm, ...
- ==Evolutionary computation== algorithms works by simulating the process of natural evolution, using mechanisms such as selection, mutation, and crossover.
  - Genetic algorithm, differential evolution, ...

<div class="mt-10">

- High evaluation cost
- Slow convergence

</div>

<Image src="../hpo-genetic.svg" figureClass="fixed right-20 bottom-12" class="h-43" />


---

# Bayesian Optimization

Hyperparameter tuning involves a black-box optimization problem:
$$
x^* \in \argmax_{x \in \mathcal{X}} f(x)
$$
- No access to gradient information
- Trial and error is necessary to evaluate hyperparameter

::row{class="mt-5" justify="between"}

<Image src="../bandit.png" figureClass="w-60" class="h-25 mx-auto" alt="?" />
<Image src="../bandit.png" figureClass="w-60" class="h-25 mx-auto" alt="?" />
<Image src="../bandit.png" figureClass="w-60" class="h-25 mx-auto" alt="?" />

::

---

# Bayesian Optimization

::row{class="mt-5"}

<Image src="../bandit.png" figureClass="w-60" class="h-20 mx-auto" />
<Image src="../bandit.png" figureClass="w-60" class="h-25 mx-auto" />
<Image src="../bandit.png" figureClass="w-60" class="h-15 mx-auto" />

::
::row{class="text-base"}

<div class="text-center w-60 mb-12">

0€\
1€\
0€\
1€\
0€\
1€

</div>
<div class="text-center w-60">

3€\
0€\
4€\
1€\
2€\
3€\
0€\
1€

</div>
<div class="text-center w-60 mb-36">

2€\
1€

</div>

::

- Sequential search, unlike parallel search, incorporates previous observations.


---

# Bayesian Optimization

::row{class="mt-5"}

<Image src="../bandit.png" figureClass="w-60" class="h-20 mx-auto" />
<Image src="../bandit.png" figureClass="w-60" class="h-25 mx-auto" />
<Image src="../bandit.png" figureClass="w-60" class="h-15 mx-auto" />

::
::row{class="text-base"}

<div class="text-center w-60">

$$
\begin{gathered}  
\hat{\mu}_1^{(15)} = 2 / 5 = 0.4\\
n_1^{(15)} = 5
\end{gathered}
$$

</div>
<div class="text-center w-60">

$$
\begin{gathered}  
\hat{\mu}_2^{(15)} = 14 / 8 = 1.75\\
n_2^{(15)} = 8
\end{gathered}
$$

</div>
<div class="text-center w-60">

$$
\begin{gathered}  
\hat{\mu}_3^{(15)} = 3 / 2 = 1.5\\
n_3^{(15)} = 2
\end{gathered}
$$

</div>

::

- To improve the searching efficiency, we have to balance **exploitation** (observation) and **exploration** (uncertainty).

==Bayesian optimization (BO)== works by building a surrogate function of the unknown objective, and manage exploitation-exploration trade-off using Bayesian methods.


---

# Bayesian Optimization

Consider a black-box optimization problem:
$$
x^* \in \argmax_{x \in \mathcal{X}} f(x)
$$

**Properties**:
- The input $x$ is in $\mathbb{R}^d$, where $d$ is ++**not too large**++, typically $d<20$.
- The feasible set $\mathcal{X}$ is a simple set, in which it is easy to assess membership.
- The true objective function $f$ is a ++**black-box**++ and ++**expensive to evaluate**++. It is also continuous and lacks known special structure like concavity or linearity.
- Focusing on finding ++**a global optimum**++ rather than a local one.

---

# Bayesian Optimization

<Bochartempty/>


---

# Bayesian Optimization

<Image src="../bo-flow.svg" figureClass="w-full mb-5" class="h-36 mx-auto" />

Main components of Bayesian optimization:
- ==Bayesian statistical model==: 
  - Approximate the objective function
- ==Acquisition function==: 
  - Determine the next point to sample


---

# Bayesian Optimization

<Svgslider :svgs="svgPathsEI" />

<script setup>
  const svgPathsEI = [
    '../bo_EI_0.svg',
    '../bo_EI_1.svg',
    '../bo_EI_2.svg',
    '../bo_EI_3.svg',
    '../bo_EI_4.svg',
    '../bo_EI_5.svg',
    '../bo_EI_6.svg',
    '../bo_EI_7.svg',
    '../bo_EI_8.svg',
  ];
</script>

---

# Bayesian Optimization

- ==Gaussian process regression (GPR)== is the most common choice of the Bayesian statistical model in BO. It is based on **GP**, which is a ++stochastic process++ where any finite set of random variables follows a multivariate Gaussian distribution.
  - Finite dimensional: multivariate Gaussian distribution, characterized by a **mean vector** $\bm{\mu}$ and a **covariance matrix** $\bm{K}$.
    $$
    \bm{X} \sim \mathcal{N}(\bm{\mu},\bm{K})
    $$
  - Infinite dimensional: Gaussian process, characterized by a **mean function** $m(\cdot)$ and a **kernel/covariance function** $\kappa(\cdot, \cdot)$.
    $$
    f(\bm{x}) \sim \mathcal{GP}(m(\bm{x}), \kappa(\bm{x}, \cdot))
    $$

---

# Bayesian Optimization

<div class="px-5 my-5 bg-orange-50 rounded-md border-2 border-orange-100 text-orange-800">

The kernel function is a **prior** of GP.

</div>

The **white kernel** assumes independence between different points.

<div class="-mt-3">

$$
\kappa_{\text{white}}(x,z) = \bm{I}_{x}
$$

</div>

<Image src="../gp_samples_white.gif" figureClass="fixed left-12 top-70" class="h-57" />
<Image src="../kernel_mat_white.svg" figureClass="fixed right-12 top-70" class="h-53" />


---

# Bayesian Optimization

The **RBF kernel** imposes a smooth prior on the sampled functions, parameterized by a length scale coefficient $l$.

$$
\kappa_{\text{rbf}}(x,z) = \exp \left( -\frac{\lVert x - z \rVert^2}{2l^2} \right)
$$

<Image src="../gp_samples_rbf.gif" figureClass="fixed left-12 top-70" class="h-57" />
<Image src="../kernel_mat_rbf.svg" figureClass="fixed right-12 top-70" class="h-53" />


---

# Bayesian Optimization

The **periodic kernel** imposes a smooth periodic prior on the sampled functions, parameterized by a length scale coefficient $l$ and periodicity $p$.

$$
\kappa_{\text{periodic}}(x,z) = \exp \left( -\frac{2}{l^2} \sin^2 \left( \frac{\lVert x - z \rVert}{p/\pi} \right) \right)
$$

<Image src="../gp_samples_periodic.gif" figureClass="fixed left-12 top-70" class="h-57" />
<Image src="../kernel_mat_periodic.svg" figureClass="fixed right-12 top-70" class="h-53" />


---

# Bayesian Optimization

The **linear kernel** imposes a linear non-stationary prior on the sampled functions, parameterized by an offset $c$.

$$
\kappa_{\text{linear}}(x,z) = (x - c)'(z - c)
$$

<Image src="../gp_samples_linear.gif" figureClass="fixed left-12 top-70" class="h-57" />
<Image src="../kernel_mat_linear.svg" figureClass="fixed right-12 top-70" class="h-53" />

---

# Bayesian Optimization

Given the GP with prior mean and kernel function: $f(\bm{x}) \sim \mathcal{GP}(m(\bm{x}), \kappa(\bm{x}, \cdot))$, the observed data follows the multivariate Gaussian distribution:
$$
\bm{Y} \sim \mathcal{N}(\bm{\mu}, \bm{K})
$$

where
$$
\bm{\mu} = \begin{pmatrix}
  m(x_1) \\ m(x_2) \\ \vdots \\ m(x_n)
\end{pmatrix}, 
\bm{K} = \begin{pmatrix}
  \kappa(x_1, x_1) & \kappa(x_1, x_2) & \ldots & \kappa(x_1, x_n) \\
  \kappa(x_2, x_1) & \kappa(x_2, x_2) & \ldots & \kappa(x_2, x_n) \\
  \vdots & \vdots & \ddots & \vdots \\
  \kappa(x_n, x_1) & \kappa(x_n, x_2) & \ldots & \kappa(x_n, x_n)
\end{pmatrix}
$$

> A common choice of prior mean is constant zero or constant mean.


---

# Bayesian Optimization

For the random variable $Y_*$ corresponding to an arbitrary point $x_*$, it follows a **joint multivariate Gaussian distribution with observed data**:
$$
\begin{pmatrix}
  \bm{Y} \\ Y_*
\end{pmatrix} \sim
\mathcal{N} \left(
  \begin{pmatrix}
    \bm{\mu} \\ \mu_*
  \end{pmatrix},
  \begin{pmatrix}
    \bm{K} & \bm{\kappa}_* \\
    \bm{\kappa}'_* & \kappa_{**}
  \end{pmatrix}
\right)
$$


The inference of GPR is to compute the **posterior distribution** of GP, which can be computed by the marginalizing the joint distribution above on the observed data:
$$
\begin{aligned}
  Y_* \vert \bm{Y}, \bm{X}, X_* &\sim \mathcal{N} (m_*, \Sigma_*) \\
  m_* &= \mu_* + \bm{\kappa}'_* \bm{K}^{-1} (\bm{y} - \bm{\mu}) \\
  \Sigma_* &= \kappa_{**} - \bm{\kappa}'_* \bm{K}^{-1} \bm{\kappa}_*
\end{aligned}
$$

<div class="-mt-3">

where $\bm{\kappa}_* = (\kappa(x_*,x_1), \ldots, \kappa(x_*,x_n))'$, $\kappa_{**} = \kappa(x_*,x_*)$.

</div>


---

# Bayesian Optimization

<v-clicks>

<div>

- **Handling observational noises**:
  - Apply white noise on the kernel matrix of observed data:
  $$
  \begin{pmatrix}
    \bm{Y} \\ Y_*
  \end{pmatrix} \sim
  \mathcal{N} \left(
    \begin{pmatrix}
      \bm{\mu} \\ \mu_*
    \end{pmatrix},
    \begin{pmatrix}
      \textcolor{coral}{\bm{K} + \alpha \bm{I}} & \bm{\kappa}_* \\
      \bm{\kappa}'_* & \kappa_{**}
    \end{pmatrix}
  \right)
  $$

</div>
<div>

- **Tuning parameters of kernel functions**:
  - Maximize the log marginal likelihood:
  $$
  \max_\theta \log p(\bm{Y} | \bm{X}, \theta) = 
  - \frac{1}{2} \textcolor{crimson}{ \underbrace{ \bm{Y}^\top (\bm{K} + \alpha \bm{I})^{-1} \bm{Y} }_{ \text{fitting} } }
  - \frac{1}{2} \textcolor{royalblue}{ \underbrace{ \log |\bm{K} + \alpha \bm{I}| }_{ \text{model complexity} } }
  - \frac{n}{2} \log 2\pi
  $$

</div>

</v-clicks>


---

# Bayesian Optimization

::row 

- **Acquisition function** is a function of the posterior distribution to ++trade-off between exploration and exploitation++.
  - To better **exploit** what we have known, we want to choose a point with high **posterior mean**.
  - To better **explore** what we don't know, we want to choose a point with high **uncertainty**.

<Image src="../bo.png" figureClass="w-100 mb-10" alt="Source: Bischl et al. (2021) Hyperparameter Optimization: Foundations, Algorithms, Best Practices and Open Challenges" />

::

---

# Bayesian Optimization

==Expected Improvement (EI)== is the expectation of the improvement brought by observing a new point.
$$
A_{\text{EI}}(x) = \mathbb{E} \left( \max(0, f(x) - f(x^+)) \right)
$$

There is an analytical expression:
$$
\begin{aligned}
  A_{\text{EI}}(x) &= \begin{cases}
  \left( \mu_t(x) - f(x^+) - \epsilon \right) \Phi(Z) + \sigma_t(x) \phi(Z), & \text{if } \sigma_t(x) > 0\\
  0, & \text{if } \sigma_t(x) = 0
\end{cases} \\
  Z &= \frac{\mu_t(x) - f(x^+) - \epsilon}{\sigma_t(x)}
\end{aligned}
$$
where $\Phi(\cdot)$ and $\phi(\cdot)$ are the cdf and pdf of standard Gaussian distribution.


---

# Bayesian Optimization

<Svgslider :svgs="svgPathsEI" />

<script setup>
  const svgPathsEI = [
    '../bo_EI_0.svg',
    '../bo_EI_1.svg',
    '../bo_EI_2.svg',
    '../bo_EI_3.svg',
    '../bo_EI_4.svg',
    '../bo_EI_5.svg',
    '../bo_EI_6.svg',
    '../bo_EI_7.svg',
    '../bo_EI_8.svg',
  ];
</script>


---

# Bayesian Optimization

==Upper confidence bound (UCB)== is a linear combination of the posterior mean and standard deviation.
$$
A_{\text{UCB}}(x) = \mu(x) + \sqrt{\beta} \sigma(x)
$$

<Svgslider :svgs="svgPaths" classes="w-180 mx-auto -mt-10" controlsClasses="-mt-5" />

<script setup>
  const svgPaths = [
    '../bo_h_UCB_0.svg',
    '../bo_h_UCB_1.svg',
    '../bo_h_UCB_2.svg',
    '../bo_h_UCB_3.svg',
    '../bo_h_UCB_4.svg',
    '../bo_h_UCB_5.svg',
    '../bo_h_UCB_6.svg',
    '../bo_h_UCB_7.svg',
    '../bo_h_UCB_8.svg',
  ];
</script>


---

# Bayesian Optimization

==Thompson sampling== selects the next point by iteratively sampling from the posterior distribution.

<Svgslider :svgs="svgPaths" classes="w-180 mx-auto -mt-1" controlsClasses="-mt-5" />

<script setup>
  const svgPaths = [
    '../bo_h_Thompson_0.svg',
    '../bo_h_Thompson_1.svg',
    '../bo_h_Thompson_2.svg',
    '../bo_h_Thompson_3.svg',
    '../bo_h_Thompson_4.svg',
    '../bo_h_Thompson_5.svg',
    '../bo_h_Thompson_6.svg',
    '../bo_h_Thompson_7.svg',
    '../bo_h_Thompson_8.svg',
  ];
</script>