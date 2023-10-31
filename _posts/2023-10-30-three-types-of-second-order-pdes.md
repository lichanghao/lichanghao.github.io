---
title:             "Three different types of linear second order PDEs"
date:              2023-10-30
permalink:         /posts/2023/10/30/three-types-of-second-order-pdes
tags:              Research
---

This post is a note on the characteristics of the elliptic, hyperbolic, and parabolic PDEs.

# General form of second order PDEs

Consider the most general form of the linear second order PDEs:

$$ a_{11}u_{xx} + 2a_{12}u_{xy} + a_{22}u_{yy} + b_1u_x + b_2u_y + cu = f. \label{eq1}\tag1$$

We then see if we can transform the Equation(\ref{eq1}) into simpler form. Introduce the substitution of variables

$$ \xi = \xi(x,y), \eta = \eta(x,y), \label{eq2}\tag2 $$

where the jacobian determinant of the transformation $\frac{D(\xi, \eta)}{D(x, y)} = \left\| \begin{matrix} \xi_x & \xi_y \\ \eta_x & \eta_y \end{matrix} \right\| > 0$ near the given point $(x_0, y_0)$. We call this substitution reversible if the above condition is satisfied. Apply the transformation to the original PDE Equation(\ref{eq1}), we have

$$ \bar{a}_{11}u_{\xi\xi} + 2\bar{a}_{12}u_{\xi\eta} + \bar{a}_{22}u_{\eta\eta} + \bar{b}_1u_{\xi} + \bar{b}_2u_{\eta} + \bar{c}u = \bar{f}. \label{eq3}\tag3$$

It is obvious that

$$  \begin{cases} \bar{a}_{11} = a_{11}\xi_x^2 + 2a_{12}\xi_x\xi_y + a_{22}\xi_y^2  \\
                  \bar{a}_{12} = a_{11}\xi_x\eta_x + a_{12}(\xi_x\eta_y + \xi_y\eta_x) + a_{22}\xi_y\eta_y \\
                  \bar{a}_{22} = a_{11}\eta_x^2 + 2a_{12}\eta_x\eta_y + a_{22}\eta_y^2 \end{cases}, \label{eq4}\tag4 $$

and $\bar{b}_1, \bar{b}_2, \bar{c}, \bar{f}$ can be similarly obtained using the chain rule.

One can observe that in Equation(\ref{eq4}), the first equation and the third equation share the same formulation, which we call the characteristic equation.