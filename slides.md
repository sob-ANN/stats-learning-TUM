---
theme: tum-unofficial

event: Statistical Learning and Data Analytics for Transportation Systems
short_event: Statistical Learning and Data Analytics
title: Kernel Machines
footer: true
date: 05/06/2026

author: Soban Lone
email: soban.lone@tum.de
institute: Technical University of Munich
department: TUM School of Engineering and Design
chair: Chair of Transportation Systems Engineering
location: Munich

css: unocss
drawings:
  persist: false
transition: fade
mdc: true
fonts:
  provider: google
  sans: Rubik
  serif: Petrona
  weight: '200;400;500;600;700'
  italic: true
---

<!--
Hello

This time, Support vector machine.
Frankly speaking, SVM is almost a model that no one use nowadays because we've got many more efficient and more effective alternative models. 

But there are many ideas of model design derived from SVM that are still widely used across various models. So it's good to know about the design philosophy into this model. 

Before the start, I would also like to remind you that SVM is a mathematically intensive model. But don't worry if you find yourself hard to follow. Just remember to grasp the main ideas. That's enough.
-->

---

# Recap: Linear Foundations & The Path to Kernels

<!-- ### 🔄 The Journey So Far -->
* **Linear Regression & Regularization**: Predicting continuous outputs while controlling model complexity (L1/L2).
<!-- * **Multiple Regression & Model Selection**: Scaling to multi-feature inputs while using L1/L2 regularization to balance complexity and generalization. -->
* **Dimensionality Reduction**: Projecting complex, high-dimensional data onto major axes of variance.
* **Time Series**: Extracting sequential patterns and tracking temporal dependencies.
* **Linear Classification**: Partitioning feature spaces using rigid, straight-line boundaries.

## The Motivation for Kernel Methods
* **The Linear Limit**: Real-world data structures are rarely straight (e.g., circles, XOR).
<!-- * **High-Dimensional Mapping**: Moving data into higher dimensions makes non-linear data linearly separable. -->
* **The Kernel Trick**: Accomplishing this mapping implicitly by swapping dot products for similarity functions.

---
layout: section
section: Linear SVM
---

---
src: ./pages/linear.md
---


---
layout: section
section: Soft Margin SVM
---

---
src: ./pages/soft.md
---


---
layout: section
section: Kernel Trick
---

---
src: ./pages/kernel.md
---


---
layout: section
section: Support Vector Regression
---

---
src: ./pages/svr.md
---


---
layout: section
section: Hyperparameter Optimization and Gaussian Process
---

---
src: ./pages/hpo.md
---

---
layout: ending
---
