# Exercise 2 - Potential Flow Around an Obstacle

We denote by

$$
\mathcal{O} = \Omega \setminus \overline{D}
$$

the fluid domain. The stream function $\psi$ satisfies

$$
\begin{cases}
-\Delta \psi = 0 & \text{in } \mathcal{O}, \\
\partial_n \psi = -v_y n_x + v_x n_y & \text{on } \partial \Omega, \\
\psi = 0 & \text{on } \partial D.
\end{cases}
$$

The velocity is defined by

$$
u = (u_x,u_y) = \left(\partial_y \psi, -\partial_x \psi\right).
$$

## 1. Explanation of the first two equations

### Equation inside the domain

The fluid is incompressible and irrotational.

First, with the definition of the stream function,

$$
\nabla \cdot u = \partial_x(\partial_y \psi) + \partial_y(-\partial_x \psi) = 0,
$$

so incompressibility is automatically satisfied.

For the vorticity in 2D, we compute

$$
\partial_x u_y - \partial_y u_x = \partial_x(-\partial_x \psi) - \partial_y(\partial_y \psi) = -\Delta \psi.
$$

Since the flow is irrotational, this quantity must vanish, hence

$$
-\Delta \psi = 0 \quad \text{in } \mathcal{O}.
$$

### Boundary condition on $\partial \Omega$

On the outer boundary, the velocity is prescribed and equal to $v=(v_x,v_y)$. We must therefore rewrite this condition in terms of $\psi$.

Let $n=(n_x,n_y)$ be the outward unit normal vector. Then

$$
\partial_n \psi = \nabla \psi \cdot n = \partial_x \psi \, n_x + \partial_y \psi \, n_y.
$$

Using

$$
u_x = \partial_y \psi, \qquad u_y = -\partial_x \psi,
$$

we obtain

$$
\partial_n \psi = -u_y n_x + u_x n_y.
$$

Since $u=v$ on $\partial \Omega$, this gives

$$
\partial_n \psi = -v_y n_x + v_x n_y \quad \text{on } \partial \Omega.
$$

Finally, on the obstacle boundary $\partial D$, the condition $\psi=0$ means that $\psi$ is constant there, hence its tangential derivative vanishes. This implies $u \cdot n = 0$, which is the slip condition.

## 2. Variational formulation

We consider the space

$$
V = \{\varphi \in H^1(\mathcal{O}) \; ; \; \varphi = 0 \text{ on } \partial D\}.
$$

Let $\varphi \in V$. Multiplying the equation by $\varphi$ and integrating over $\mathcal{O}$ gives

$$
\int_{\mathcal{O}} (-\Delta \psi) \varphi \, dxdy = 0.
$$

Applying Green's formula,

$$
\int_{\mathcal{O}} \nabla \psi \cdot \nabla \varphi \, dxdy
- \int_{\partial \mathcal{O}} \partial_n \psi \, \varphi \, ds = 0.
$$

Now

$$
\partial \mathcal{O} = \partial \Omega \cup \partial D.
$$

On $\partial D$, $\varphi=0$, so the contribution is zero. On $\partial \Omega$, we use the Neumann condition. Therefore

$$
\int_{\mathcal{O}} \nabla \psi \cdot \nabla \varphi \, dxdy
= \int_{\partial \Omega} (-v_y n_x + v_x n_y) \varphi \, ds.
$$

The weak formulation is:

$$
\text{Find } \psi \in V \text{ such that}
$$

$$
\int_{\mathcal{O}} \nabla \psi \cdot \nabla \varphi \, dxdy
= \int_{\partial \Omega} (-v_y n_x + v_x n_y) \varphi \, ds
\quad \forall \varphi \in V.
$$

This problem is well posed because the Dirichlet condition on $\partial D$ removes the additive constant indeterminacy of the Laplace equation and yields coercivity on $V$.

## 3. Geometry and mesh

The statement only fixes the shape of the domain. In [solution.edp](solution.edp), I use:

- the unit square for the outer box $\Omega$,
- a centered diamond obstacle to match the figure,
- a second test with a circular obstacle for question 6.

Both meshes are created only with `border` and `buildmesh`, exactly as in the previous practical sessions.

## 4. FreeFem++ resolution

The script [solution.edp](solution.edp) solves three cases:

1. diamond obstacle with horizontal profile $v=(1,0)$,
2. diamond obstacle with vertical profile $v=(0,1)$,
3. circular obstacle with horizontal profile $v=(1,0)$.

In each case, the variational formulation is implemented with a `problem`, the obstacle boundary is imposed with `on(...)`, and the outer boundary condition is introduced with `int1d(...)` using the formula

$$
g = -v_y n_x + v_x n_y.
$$

## 5. Plot adapted to the meaning of the stream function

Since the streamlines are the isovalues of $\psi$, the most relevant plot is a contour-type plot rather than only a filled scalar map. In the script, I therefore use `plot` without filling to emphasize the level lines of $\psi$.

## 6. Comments on the results

For the horizontal profile $v=(1,0)$, the streamlines are globally horizontal far from the obstacle and bend around the diamond while remaining tangent to its boundary. The obstacle creates a symmetric disturbance above and below the center.

When the imposed profile is rotated to $v=(0,1)$, the picture rotates accordingly: the streamlines become globally vertical far from the obstacle and adapt around it with the same symmetry principle.

When the diamond is replaced by a circular obstacle, the deviation of the streamlines becomes smoother because the boundary curvature is smooth, unlike the diamond that creates stronger directional effects near its corners.

The executions of [solution.edp](solution.edp) give the following Dirichlet energies:

$$
\int_{\mathcal{O}} |\nabla \psi|^2 \approx 0.938429
$$

for the diamond with horizontal flow,

$$
\int_{\mathcal{O}} |\nabla \psi|^2 \approx 0.938401
$$

for the diamond with vertical flow, and

$$
\int_{\mathcal{O}} |\nabla \psi|^2 \approx 0.868681
$$

for the circular obstacle with horizontal flow. The first two values are almost identical, which matches the rotational symmetry of the setup when the velocity direction is rotated by $\pi/2$.