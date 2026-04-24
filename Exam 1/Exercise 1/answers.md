# Exercise 1 - Poisson Problem

We consider

$$
\begin{cases}
-\nabla \cdot (\kappa \nabla u) + b u = f & \text{in } \Omega, \\
u = c & \text{on } \Gamma_D, \\
\partial_n u = 0 & \text{on } \Gamma_N,
\end{cases}
$$

with $\Omega = ]0,5[ \times ]-2,2[$, $b \in \mathbb{R}_+$, $c \in \mathbb{R}$, $f \in L^2(\Omega)$, $\kappa \in L^\infty(\Omega)$, and

$$
0 < \kappa_- \le \kappa(x,y) \le \kappa_+.
$$

## 1. Variational formulation for the case $c=0$

Since the Dirichlet datum is homogeneous, the natural space is

$$
V = \{v \in H^1(\Omega) \; ; \; v = 0 \text{ on } \Gamma_D\}.
$$

We first make the formal derivation with smooth functions. Let $v \in V$. Multiplying the equation by $v$ and integrating over $\Omega$ gives

$$
\int_\Omega \bigl(-\nabla \cdot (\kappa \nabla u)\bigr) v \, dxdy + \int_\Omega b u v \, dxdy = \int_\Omega f v \, dxdy.
$$

Using Green's formula,

$$
\int_\Omega \bigl(-\nabla \cdot (\kappa \nabla u)\bigr) v \, dxdy
= \int_\Omega \kappa \nabla u \cdot \nabla v \, dxdy
- \int_{\partial \Omega} \kappa \, \partial_n u \, v \, ds.
$$

Now we split the boundary term:

$$
\int_{\partial \Omega} \kappa \, \partial_n u \, v \, ds
= \int_{\Gamma_D} \kappa \, \partial_n u \, v \, ds
+ \int_{\Gamma_N} \kappa \, \partial_n u \, v \, ds.
$$

On $\Gamma_D$, $v=0$, hence the first term is zero. On $\Gamma_N$, we have $\partial_n u = 0$, hence the second term is also zero. Therefore,

$$
\int_\Omega \kappa \nabla u \cdot \nabla v \, dxdy + \int_\Omega b u v \, dxdy = \int_\Omega f v \, dxdy.
$$

We define

$$
a(u,v) = \int_\Omega \kappa \nabla u \cdot \nabla v \, dxdy + \int_\Omega b u v \, dxdy,
$$

$$
\ell(v) = \int_\Omega f v \, dxdy.
$$

The weak formulation is then:

$$
\text{Find } u \in V \text{ such that } a(u,v) = \ell(v) \quad \forall v \in V.
$$

This formulation is well posed. Indeed, $a$ is continuous on $V \times V$, and since $\kappa \ge \kappa_- > 0$ and $\Gamma_D$ has positive measure, the Poincare inequality holds on $V$, so

$$
a(v,v) = \int_\Omega \kappa |\nabla v|^2 \, dxdy + \int_\Omega b v^2 \, dxdy
\ge \kappa_- \|\nabla v\|_{L^2(\Omega)}^2
\ge C \|v\|_{H^1(\Omega)}^2.
$$

Hence Lax-Milgram applies.

## 2. A function satisfying the boundary conditions

Here $\Gamma_D$ is made of the two vertical sides $x=0$ and $x=5$, and $\Gamma_N$ is made of the two horizontal sides $y=-2$ and $y=2$.

We choose

$$
u(x,y) = c + \sin\left(\frac{\pi x}{5}\right) \cos\left(\frac{\pi y}{2}\right).
$$

This function is not constant in $x$ and not constant in $y$.

On $x=0$ and $x=5$,

$$
\sin\left(\frac{\pi x}{5}\right) = 0,
$$

so

$$
u(0,y) = u(5,y) = c.
$$

Moreover,

$$
\partial_y u(x,y) = -\frac{\pi}{2} \sin\left(\frac{\pi x}{5}\right) \sin\left(\frac{\pi y}{2}\right).
$$

Hence on $y=\pm 2$,

$$
\sin\left(\frac{\pi y}{2}\right) = \sin(\pm \pi) = 0,
$$

therefore $\partial_y u(x,\pm 2)=0$, and thus $\partial_n u=0$ on the two horizontal borders.

So this $u$ satisfies the prescribed boundary conditions.

## 3. Choice of $f$ when $\kappa$ is constant

Assume $\kappa$ is constant. We compute

$$
\partial_{xx} u = -\left(\frac{\pi}{5}\right)^2 \sin\left(\frac{\pi x}{5}\right) \cos\left(\frac{\pi y}{2}\right),
$$

$$
\partial_{yy} u = -\left(\frac{\pi}{2}\right)^2 \sin\left(\frac{\pi x}{5}\right) \cos\left(\frac{\pi y}{2}\right).
$$

Hence

$$
\Delta u = -\left[\left(\frac{\pi}{5}\right)^2 + \left(\frac{\pi}{2}\right)^2\right]
\sin\left(\frac{\pi x}{5}\right) \cos\left(\frac{\pi y}{2}\right).
$$

Therefore

$$
-\nabla \cdot (\kappa \nabla u) + b u = -\kappa \Delta u + b u,
$$

so a suitable right-hand side is

$$
f(x,y) = bc + \left[\kappa \left(\left(\frac{\pi}{5}\right)^2 + \left(\frac{\pi}{2}\right)^2\right) + b\right]
\sin\left(\frac{\pi x}{5}\right) \cos\left(\frac{\pi y}{2}\right).
$$

For the numerical part we take $b=c=2$ and $\kappa=0.5$.

## 4. Numerical resolution with FreeFem++

The script [solution.edp](solution.edp) uses:

- a rectangular mesh of $\Omega$ built with `border` and `buildmesh`,
- a $P1$ finite element space,
- the exact function chosen above,
- the corresponding right-hand side for $b=c=2$ and $\kappa=0.5$,
- the weak formulation with Dirichlet conditions on the two vertical sides.

## 5. $H^1$ error

The script computes

$$
\|u-u_h\|_{H^1(\Omega)} = \left(\int_\Omega |u-u_h|^2 + |\nabla(u-u_h)|^2\, dxdy\right)^{1/2}.
$$

After execution of [solution.edp](solution.edp), we obtain

$$
\|u-u_h\|_{L^2(\Omega)} \approx 2.28377 \times 10^{-3},
$$

$$
|u-u_h|_{H^1(\Omega)} \approx 3.22474 \times 10^{-2},
$$

$$
\|u-u_h\|_{H^1(\Omega)} \approx 3.23282 \times 10^{-2}.
$$

The approximation is therefore consistent with the exact smooth solution chosen above.

## 6. Piecewise coefficient $\kappa$

We then replace the constant diffusion coefficient by

$$
\kappa(x,y) =
\begin{cases}
50 & \text{if } (x,y) \in ]1,4[ \times ]-1,1[, \\
0.5 & \text{otherwise.}
\end{cases}
$$

With this coefficient, the medium becomes much more conductive in the central rectangle. For the same source term and the same Dirichlet data, the solution becomes flatter inside that high-conductivity zone, while larger transitions are pushed near the interfaces between the two materials and near the boundaries. The exact trigonometric solution used for the constant-coefficient case is no longer the exact solution of the variable-coefficient problem, so the comparison is now qualitative rather than exact.

## 7. What happens when $b=0$?

No singularity is expected here. The diffusion term alone remains coercive on

$$
V = \{v \in H^1(\Omega) ; v=0 \text{ on } \Gamma_D\}
$$

because $\kappa \ge \kappa_- > 0$ and $\Gamma_D$ has positive measure. By the Poincare inequality,

$$
\int_\Omega \kappa |\nabla v|^2 \, dxdy
\ge \kappa_- \|\nabla v\|_{L^2(\Omega)}^2
\ge C \|v\|_{H^1(\Omega)}^2.
$$

So the problem is still well posed for $b=0$. A difficulty would appear for a pure Neumann problem, but that is not the case here because the Dirichlet boundary is nonempty.