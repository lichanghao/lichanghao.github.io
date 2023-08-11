---
title:             "Poisson-Nernst-Planck-system"
date:              2023-08-10
permalink:         /posts/2023/08/10/Poisson-Nernst-Planck-system
tags:              Research
---

# Poisson-Nernst-Planck System

In batteries, fuel cells, and other natual/artificial electrochemical systems, the coupled diffusion effect and electro-migration effect of the charged particles can be modeled by coupling Nernst-Planck equation and Poisson equation. The Nernst-Planck equation can be written as

$$ \frac{\partial c_i}{\partial t} = \nabla \cdot \left\{ D_i\left[\nabla c_i + {z_ie\over{k_\text{B}T}}c_i \left( \nabla \phi + {\partial {\bf A}\over{\partial t}} \right) \right] \right\} ,$$

where $c_i$ is the concentration of the $i^{th}$ charged particle, $D_i$, $z_i$ is the corresponding diffusivity and ion valence, $\phi$ is the electrostatic potential, $\mathbf{A}$ is the magnetic field. It should be note that $\frac{e}{k_{\rm{B}}T} = \frac{F}{R T}$, the same equation in microscopic/marcoscopic views. Also, $\mathbf{A}$ can be neglected given the absence of magnetic field.



