# Green's Formulas for Variational Methods

## 1. 1D Scalar Problem (ODE / Heat Bar)
* **Problem Type:** Poisson / Diffusion in 1D.
* **Dimensions:** 1D.
* **Number of Unknowns:** 1 ($u$).
* **Formula:**
    $$\int_a^b -u''(x) v(x) \, dx = \int_a^b u'(x) v'(x) \, dx - \Big[ u'(x) v(x) \Big]_a^b$$

---

## 2. 2D/3D Scalar Problem (Laplace / Poisson)
* **Problem Type:** Steady-state heat, Electrostatics, Pressure.
* **Dimensions:** 2D or 3D.
* **Number of Unknowns:** 1 ($u$).
* **Formula (Green's First Identity):**
    $$\int_\Omega -\Delta u \, v \, d\Omega = \int_\Omega \nabla u \cdot \nabla v \, d\Omega - \int_{\partial\Omega} \frac{\partial u}{\partial n} v \, d\Gamma$$
    *Where $\frac{\partial u}{\partial n} = \nabla u \cdot \mathbf{n}$ is the normal derivative.*

---

## 3. 2D/3D General Diffusion (Variable Coefficient)
* **Problem Type:** Darcy Flow, Anisotropic Heat.
* **Dimensions:** 2D or 3D.
* **Number of Unknowns:** 1 ($u$).
* **Formula:**
    $$\int_\Omega -\nabla \cdot (\alpha \nabla u) v \, d\Omega = \int_\Omega \alpha \nabla u \cdot \nabla v \, d\Omega - \int_{\partial\Omega} (\alpha \nabla u \cdot \mathbf{n}) v \, d\Gamma$$

---

## 4. 2D/3D Vector Problem (Linear Elasticity)
* **Problem Type:** Structural mechanics (displacements).
* **Dimensions:** 2D or 3D.
* **Number of Unknowns:** 2 (2D) or 3 (3D) ($\mathbf{u} = (u_1, u_2, ...)$).
* **Formula:**
    $$\int_\Omega -(\nabla \cdot \boldsymbol{\sigma}) \cdot \mathbf{v} \, d\Omega = \int_\Omega \boldsymbol{\sigma} : \boldsymbol{\varepsilon}(\mathbf{v}) \, d\Omega - \int_{\partial\Omega} (\boldsymbol{\sigma} \mathbf{n}) \cdot \mathbf{v} \, d\Gamma$$
    *Note: $\boldsymbol{\sigma} : \boldsymbol{\varepsilon}(\mathbf{v})$ is the double contraction of the stress and strain tensors.*

---

## 5. 2D/3D Pressure-Velocity Gradient (Stokes / Mixed Methods)
* **Problem Type:** Fluid mechanics (coupling pressure and velocity).
* **Dimensions:** 2D or 3D.
* **Number of Unknowns:** Scalar $p$ and Vector $\mathbf{u}$.
* **Formula (Integration of Gradient):**
    $$\int_\Omega \nabla p \cdot \mathbf{v} \, d\Omega = -\int_\Omega p (\nabla \cdot \mathbf{v}) \, d\Omega + \int_{\partial\Omega} p (\mathbf{v} \cdot \mathbf{n}) \, d\Gamma$$

---

## 6. 1D Beam Theory (Fourth Order)
* **Problem Type:** Euler-Bernoulli Beam.
* **Dimensions:** 1D.
* **Number of Unknowns:** 1 ($w$, deflection).
* **Formula:**
    $$\int_0^L w''''(x) v(x) \, dx = \int_0^L w''(x) v''(x) \, dx + \Big[ w''' v - w'' v' \Big]_0^L$$

---

## Summary of Terms

| Term | Integration by Parts Result | Physical Meaning |
| :--- | :--- | :--- |
| **$-\Delta u$** | $\nabla u \cdot \nabla v$ | Diffusion / Internal Energy |
| **$\nabla p$** | $-p \text{ div}(\mathbf{v})$ | Pressure work |
| **$-\text{div}(\boldsymbol{\sigma})$** | $\boldsymbol{\sigma} : \boldsymbol{\varepsilon}(\mathbf{v})$ | Internal virtual work |
| **Boundary Term** | Flux or Traction | Interaction with exterior |