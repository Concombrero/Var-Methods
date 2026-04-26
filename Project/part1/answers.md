# Project - Part 1

We study the direct heat problem in the oven.

The domain is

$$
\Omega = ]-1.5,1.5[ \times ]-1,1[.
$$

The cooking area is the centered rectangle

$$
S = ]-0.75,0.75[ \times ]-0.5,0.5[.
$$

The six resistors are circles of radius $0.05$ centered at the points

$$
\{-1,0,1\} \times \{-0.75,0.75\}.
$$

The stationary temperature satisfies

$$
-\operatorname{div}(k\nabla T) = f \qquad \text{in } \Omega,
$$

with

$$
k(x,y) =
\begin{cases}
1 & \text{in } S, \\
10 & \text{in } \Omega \setminus S,
\end{cases}
$$

and

$$
f(x,y) = \sum_{i=1}^6 \alpha_i \mathbf{1}_{C_i}(x,y).
$$

The boundary conditions are:

- insulated left and right walls,
- top wall at temperature $T_c = 343\,\mathrm{K}$,
- bottom wall with Robin condition
  $$
  \partial_n T + h(T-T_e)=0,
  $$
  where $h=1$ and $T_e = 293\,\mathrm{K}$.

In the numerical illustration below, I take for the known coefficients

$$
\alpha_1 = \cdots = \alpha_6 = 2000.
$$

## 1(a) Variational formulation and analysis

We denote:

- $\Gamma_D$ the top boundary,
- $\Gamma_R$ the bottom boundary,
- $\Gamma_N$ the union of the left and right boundaries.

Because the Dirichlet condition is nonhomogeneous, the natural affine space is

$$
V_{T_c} = \{v \in H^1(\Omega) \; ; \; v = T_c \text{ on } \Gamma_D\},
$$

and the test space is

$$
V_0 = \{v \in H^1(\Omega) \; ; \; v = 0 \text{ on } \Gamma_D\}.
$$

Let $v \in V_0$. Multiplying the equation by $v$ and integrating over $\Omega$, we obtain

$$
\int_\Omega -\operatorname{div}(k\nabla T) v \, dx = \int_\Omega f v \, dx.
$$

Applying Green's formula gives

$$
\int_\Omega k\nabla T \cdot \nabla v \, dx - \int_{\partial\Omega} k\partial_n T \, v \, ds = \int_\Omega f v \, dx.
$$

We now split the boundary term.

On $\Gamma_D$, $v=0$, so the contribution is zero.

On $\Gamma_N$, the wall is insulated, hence $\partial_n T = 0$, so the contribution is also zero.

On $\Gamma_R$, we use

$$
\partial_n T = -h(T-T_e).
$$

Therefore

$$
-\int_{\Gamma_R} k\partial_n T \, v \, ds = \int_{\Gamma_R} kh(T-T_e)v \, ds.
$$

We finally obtain

$$
\int_\Omega k\nabla T \cdot \nabla v \, dx + \int_{\Gamma_R} khTv \, ds = \int_\Omega fv \, dx + \int_{\Gamma_R} khT_e v \, ds.
$$

Hence the weak formulation is:

$$
\text{Find } T \in V_{T_c} \text{ such that}
$$

$$
a(T,v) = \ell(v) \qquad \forall v \in V_0,
$$

with

$$
a(T,v) = \int_\Omega k\nabla T \cdot \nabla v \, dx + \int_{\Gamma_R} khTv \, ds,
$$

$$
\ell(v) = \int_\Omega fv \, dx + \int_{\Gamma_R} khT_e v \, ds.
$$

### Analysis

The bilinear form is continuous on $H^1(\Omega) \times H^1(\Omega)$ because $k \in L^\infty(\Omega)$ and the trace operator is continuous from $H^1(\Omega)$ to $L^2(\Gamma_R)$.

Moreover, for $v \in V_0$,

$$
a(v,v) = \int_\Omega k|\nabla v|^2 \, dx + \int_{\Gamma_R} khv^2 \, ds \ge \int_\Omega |\nabla v|^2 \, dx,
$$

since $k \ge 1$ and $h \ge 0$.

Because $v=0$ on the nonempty portion $\Gamma_D$ of the boundary, the Poincare inequality holds on $V_0$, so

$$
\|v\|_{H^1(\Omega)} \le C \|\nabla v\|_{L^2(\Omega)}.
$$

Therefore $a$ is coercive on $V_0$. By Lax-Milgram, the direct problem is well posed.

## 1(b) Finite element approximation and linear system

We triangulate the domain and introduce a finite element subspace $V_h \subset H^1(\Omega)$ made of $P1$ functions.

We also define

$$
V_{0h} = \{v_h \in V_h \; ; \; v_h = 0 \text{ on } \Gamma_D\}
$$

and an approximation $g_h \in V_h$ of the Dirichlet datum such that $g_h = T_c$ on $\Gamma_D$.

We look for

$$
T_h = g_h + u_h,
$$

with $u_h \in V_{0h}$ satisfying

$$
a(u_h,v_h) = \ell(v_h) - a(g_h,v_h) \qquad \forall v_h \in V_{0h}.
$$

Choosing a basis $(\varphi_j)_{j=1}^N$ of $V_{0h}$ and writing

$$
u_h = \sum_{j=1}^N U_j \varphi_j,
$$

we obtain the linear system

$$
AU = F,
$$

where

$$
A_{ij} = \int_\Omega k\nabla \varphi_j \cdot \nabla \varphi_i \, dx + \int_{\Gamma_R} kh\varphi_j\varphi_i \, ds,
$$

and

$$
F_i = \int_\Omega f\varphi_i \, dx + \int_{\Gamma_R} khT_e\varphi_i \, ds - a(g_h,\varphi_i).
$$

In FreeFem++, the Dirichlet condition is directly imposed with `on(...)`, so the assembly and the linear solve are handled automatically by the `problem` command.

## 1(c) Numerical implementation and comments

The implementation is in [solution.edp](solution.edp). It contains:

- the rectangular oven mesh,
- the piecewise coefficient $k$,
- the six circular sources,
- the stationary direct problem,
- a few integral diagnostics.

For the numerical illustration with $\alpha_i = 2000$ for all $i$, the validated script gives:

- mean temperature in $\Omega$: $324.543\,\mathrm{K}$,
- mean temperature in $S$: $324.592\,\mathrm{K}$,
- temperature at the center $(0,0)$ in the stationary regime: $323.769\,\mathrm{K}$,
- final mean temperature in $S$ for the warm-up run: $324.592\,\mathrm{K}$,
- final temperature at the center: $323.769\,\mathrm{K}$.

Qualitatively, one expects:

- a left-right symmetric stationary field, because the geometry and source intensities are symmetric with respect to the vertical axis,
- higher temperatures near the resistors,
- a hotter upper part of the oven because of the imposed temperature $T_c=343\,\mathrm{K}$,
- a cooling effect near the bottom due to the Robin boundary condition.

## 1(d) Warm-up phase and implementation

During the warm-up phase, the temperature depends on time and is governed by the heat equation

$$
\partial_t T - \operatorname{div}(k\nabla T) = f \qquad \text{in } (0,+\infty) \times \Omega,
$$

with the same boundary conditions as in the stationary case and with an initial datum. A natural choice here is a uniform initial temperature equal to the exterior temperature:

$$
T(0,x) = T_e.
$$

To discretize in time, we use the same $\theta$-scheme philosophy as in TP3. With $\theta = \tfrac12$ and time step $\Delta t$, the discrete weak formulation reads:

find $T_h^{n+1} \in V_{T_c,h}$ such that for all $v_h \in V_{0h}$,

$$
\int_\Omega T_h^{n+1} v_h \, dx
+ \theta \Delta t \int_\Omega k\nabla T_h^{n+1} \cdot \nabla v_h \, dx
+ \theta \Delta t \int_{\Gamma_R} khT_h^{n+1} v_h \, ds
$$

$$
= \int_\Omega T_h^n v_h \, dx
+ \Delta t \int_\Omega f v_h \, dx
- (1-\theta) \Delta t \int_\Omega k\nabla T_h^n \cdot \nabla v_h \, dx
- (1-\theta) \Delta t \int_{\Gamma_R} khT_h^n v_h \, ds
+ \Delta t \int_{\Gamma_R} khT_e v_h \, ds.
$$

This is also implemented in [solution.edp](solution.edp), starting from $T^0 = T_e$.

The transient solution describes how the oven temperature evolves from ambient conditions toward the stationary regime. In particular, it lets us visualize the diffusion of heat from the resistors and the gradual filling of the cooking area with heat.