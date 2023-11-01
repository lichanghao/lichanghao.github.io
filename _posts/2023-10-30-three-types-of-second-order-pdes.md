---
title:             "Three different types of linear second order PDEs"
date:              2023-10-30
permalink:         /posts/2023/10/30/three-types-of-second-order-pdes
tags:              Research
---

This post is a note on the characteristics of the elliptic, hyperbolic, and parabolic PDEs.

## The general form of second order PDEs

Consider the general form of the linear second order PDEs in two dimensions:

$$ a_{11}u_{xx} + 2a_{12}u_{xy} + a_{22}u_{yy} + b_1u_x + b_2u_y + cu = f, \label{eq1}\tag1$$

where all pre-factors and RHS are real functions of the coordinates. We then see if we can transform the Equation(\ref{eq1}) into simpler form. Introduce the substitution of variables

$$ \xi = \xi(x,y), \eta = \eta(x,y), \label{eq2}\tag2 $$

where the jacobian determinant of the transformation $\frac{D(\xi, \eta)}{D(x, y)} = \left\| \begin{matrix} \xi_x & \xi_y \\ \eta_x & \eta_y \end{matrix} \right\| > 0$ near the given point $(x_0, y_0)$. We call this substitution reversible if the above condition is satisfied. Apply the transformation to the original PDE Equation(\ref{eq1}), we have

$$ \bar{a}_{11}u_{\xi\xi} + 2\bar{a}_{12}u_{\xi\eta} + \bar{a}_{22}u_{\eta\eta} + \bar{b}_1u_{\xi} + \bar{b}_2u_{\eta} + \bar{c}u = \bar{f}. \label{eq3}\tag3$$

It is obvious that

$$  \begin{cases} \bar{a}_{11} = a_{11}\xi_x^2 + 2a_{12}\xi_x\xi_y + a_{22}\xi_y^2  \\
                  \bar{a}_{12} = a_{11}\xi_x\eta_x + a_{12}(\xi_x\eta_y + \xi_y\eta_x) + a_{22}\xi_y\eta_y \\
                  \bar{a}_{22} = a_{11}\eta_x^2 + 2a_{12}\eta_x\eta_y + a_{22}\eta_y^2 \end{cases}, \label{eq4}\tag4 $$

and $\bar{b}_1, \bar{b}_2, \bar{c}, \bar{f}$ can be similarly obtained using the chain rule.

One can observe that in Equation(\ref{eq4}), the first equation and the third equation share the same formulation, from which we construct the characteristic equation as:

$$ a_{11}\phi_x^2 + 2a_{12}\phi_x\phi_y + a_{22}\phi_y^2 = 0. \label{eq5}\tag5 $$

If we choose $\xi(x, y) = \phi_1(x, y)$ and $\eta(x, y) = \phi_2(x, y)$, where $\phi_1(x, y)$ amd $\phi_2(x, y)$ are two independent solutions of the Equation(\ref{eq5}), the Equation (\ref{3}) will be greatly simplified.


## Characteristic lines

The solution $\phi_{i}(x, y)$ of Equation (\ref{eq5}) can be integrated by:

$$\frac{dy}{dx} = \frac{a_{12} + \sqrt{a_{12}^2 - a_{11}a_{22}}}{a_{11}}, \label{eq6.1}\tag{6.1}$$

$$\frac{dy}{dx} = \frac{a_{12} - \sqrt{a_{12}^2 - a_{11}a_{22}}}{a_{11}}, \label{eq6.2}\tag{6.2}$$

and they gives two sets of implicit curves $\phi_{i}(x, y) = C$. We call $\phi_{i}(x, y) = C$ as the characteristic lines of the Equation (\ref{eq1}). Apparently, the PDE solution $u$ are identical along the characteristic lines (if such a line exists in the real space). Normally, this behavior is called "the propagation of information (boundary/initial conditions)".

From the Equation (\ref{6}) we know that real characteristic lines are not guaranteed to exist, and it depends on the sign of $a_{12}^2 - a_{11}a_{12}$. This actually determines the behavior of the given PDE.

## Hyperbolic PDEs

If $a_{12}^2 - a_{11}a_{12} > 0$, we say the PDE is hyperbolic. In this case, the PDE has two real characteristic lines, given by Equation (\ref{eq6.1}) and (\ref{eq6.2}). The solutions of hyperbolic PDEs normally have the properties of wave propagation, which means the information propagates through the characteristic lines at a finite wave speed. For example, in the wave-form solution $u = u(x+vt)$, the information propagates along the line $x+vt = C$ at a constant speed $v$.

A standard simpflication of hyperbolic PDEs is:

$$
u_{\xi\eta} = Au_{\xi} + Bu_{\eta} + Cu + D,
\label{eq9}\tag9
$$

or

$$
u_{ss} - u_{tt} = A_1u_s + B_1u_t + C_1u + D_1,
\label{eq10}\tag{10}
$$

where $u_{ss}$ is the "strain gradient" term, $u_{tt}$ is the "inertia" term, $A_1u_s$ is the "strain" term, $B_1u_t$ is the "viscosity" term, $C_1u$ is the "harmonic" term. The explanation is based on the generalized elastic viberation process.

Example: consider the following example of the forced viberation problem:

$$
\begin{cases}
\frac{\partial^2 u}{\partial t^2} - a^2 \frac{\partial^3 u}{\partial x^2} = f(x, t) \\
t = 0: u = 0, \frac{\partial u}{\partial t} = 0
\end{cases}. \label{eq7}\tag7
$$

We can directly apply the principle of linear superposition to give the solution of Equation (\ref{eq7}):

$$
u(x, t) = \frac{1}{2a} \int_{0}^{t}\int_{x-a(t-\tau)}^{x+a(t-\tau)} f(\xi, \tau) \rm{d}\xi\rm{d}\tau.
\label{eq8}\tag8
$$

It is not difficult to observe that $u(x,t)$ directly depends on the excitation function $f(x,t)$ in the "back light cone" region $\{(\xi, \tau) | 0<\tau<t \ \mathrm{and} \ x-a(t-\tau) < \xi < x+a(t-\tau) \}$. We call this region "the dependence region". On the contrary, the "front light cone" region $\{(\xi, \tau) | t<\tau \ \mathrm{and} \ x-a(t-\tau) < \xi < x+a(t-\tau) \}$ is the "influence region" of $u(x, t)$.

In the above case, the two characteristic lines $x \pm at = C$ determines the dependence region and the influence region. And the solution $u(x, t) = u(x + at)$ propagates in a constant wave speed $a$.


## Parabolic PDEs

if $a_{12}^2 - a_{11}a_{12} = 0$, we say the PDE is parabolic. In this case, the PDE only has one real characteristic line, given by

$$
\frac{dy}{dx} = -\sqrt{\frac{a_{22}}{a_{11}}}.
\label{eq11}\tag{11}
$$

The Equation (\ref{eq10}) gives one transformation $\xi = \phi_1(x, y)$. Here we choose another independent function $\eta = \phi_2(x,y)$, we get the standard simplification of the hyperbolic PDEs:

$$
u_{\eta\eta} = A_1u_{\xi} + B_1u_{\eta} + C_1u + D_1,
\label{eq12}\tag{12}
$$

where we can further introduce the following transformation $v = u\exp{(-\frac{1}{2}B_1(\xi, \tau))\mathrm{d}\tau}$ to get

$$
v_{\eta\eta} = A_2v_{\xi} + C_2v + D_2.
\label{eq13}\tag{13}
$$

We can consider $v_{\eta\eta}$ as the "diffusion" term, $A_2v_{\xi}$ as the "changing rate" term, and $C_2v$ as the "reaction" term. The explanation is based on the generalized diffusion-reaction process.

Example: consider the following heat conduction with heat source problem:

$$
\begin{cases}
\frac{\partial u}{\partial t} = a^2 \frac{\partial^2 u}{\partial x^2} + f(x, t), \\
u(x, 0) = 0.
\end{cases}
\label{eq14}\tag{14}
$$

The solution $u(x,t)$ can be given by fourier transformation:

$$
u(x,t) = \frac{1}{2a\sqrt{\pi}} \int_{0}^{t} \int_{-\infty}^{\infty} \frac{f(\xi, \tau)}{\sqrt{t - \tau}} \exp{(-\frac{(x-\xi)^2}{4a^2(t-\tau)}) \mathrm{d}\xi} \mathrm{d}\tau.
\label{eq15}\tag{15}
$$

It is obvious that the solution $u(x, t)$ does not have the form $u(x+at)$ such that the information propagates through the space with a finite wave speed $a$. Instead, the solution directly depends on the whole $0 < \tau < t$ plane. In this case, the dependence region is the lower $(x, t)$ plane and the influence region is the upper $(x, t)$ plane. We say, the speed of information propagation in infinite, as a purturbation $\delta(x-x_0,t-t_0)$ can be instaneously feeled by any point $(x, t)$ as long as $t > t_0$.

## Elliptic PDEs

if $a_{12}^2 - a_{11}a_{12} < 0$, we say the PDE is parabolic. In this case, the PDE has no real characteristic line. Here the integration of the Equation (\ref{eq6}) is complex. Assume

$$
\phi(x, y) = \phi_1(x,y) + i\phi_2(x,y) = C.
\label{eq16}\tag{16}
$$

Substitute Equation (\ref{eq16}) into Equation (\ref{eq2}), we have

$$
u_{\xi\xi} + u_{\eta\eta} = Au_{\xi} + Bu_{\eta} + Cu + D,
\label{eq17}\tag{17}
$$

where $u_{\xi\xi} + u_{\eta\eta}$ represents "diffusion" term or simply the Laplacian operator, and the RHS, $Au_{\xi} + Bu_{\eta} + Cu + D$, represents the source term together. This explanation is based on the generalized steady heat diffusion process.

Example: Consider the solution of the Laplace equation $u(x, y)$, and the famous mean-value theorem tells us that

$$
u(M_0) = -\frac{1}{4\pi} \int\int_{\Gamma_a} \left[ u\frac{\partial}{\partial\boldsymbol{n}}(\frac{1}{r}) - \frac{1}{r}\frac{\partial u}{\partial \boldsymbol{n}} \right] \mathrm{d}S \\
= -\frac{1}{4\pi} \int\int_{\Gamma_a} u \mathrm{d}S
\label{eq18}\tag{18}
$$

where $M_0$ is any point in the target omain $\Omega$ and $\Gamma_0$ is the unit spherical surface whose center is $M_0$.

In this case, the influence region and the dependence region is the whole target space $\Omega$, and the speed of information propagation is infinite.

## Underlying physics of three kinds of PDEs

According to the classification above, The wave equation is hyperbolic and time-reversible. Indeed, if you make the transformation $\tau = -t$ and substitute it into the wave equation, it will not change the form of the original PDE due to the second-order time derivative. However, the heat equation or diffusion equation is parabolic and time-irreversible because of the first-order time derivative. For the elliptic Laplace equation, it normally describes the steady state of physical processes, where the dynamic version can be both parabolic or hyperbolic.

The PDE classification can be different in different regions, depends on the value of the pre-factor functions. For example, the transonic N-S equation can be both parabolic or hyperbolic in different regions.

## Generalization to higher dimensions

We now consider the general form of the second-order linear PDE in N dimensions:

$$
a_{ij}\frac{\partial^2 u}{\partial x_i \partial x_j} + b_i \frac{\partial u}{\partial x_i} + cu = f
\label{eq19}\tag{19}
$$

We call the PDE (\ref{eq19}) elliptic if the matrix a_{ij} is positive-definite, hyperbolic if the matrix a_{ij} has $n-1$ eigenvalues with the same sign, $1$ eigenvalue with the opposite sign, and parabolic if a_{ij} is degenerated.

For other cases, we call the PDE ultrahyperbolic.