---
title:             "Physical and mathematical derivation of the speed of sound"
date:              2023-10-22
permalink:         /posts/2023/10/22/derivation-of-speed-of-sound
tags:              Research
---

This post is a note on the two methods for deriving the speed of sound.

## Speed of sound

As from its name, the speed of sound is the distance for a sound wave (elastic wave) propagates through an medium. The speed of sound is different in all kinds of the materials. Normally, the speed of sound is larger in solids than liquids, then gas. Since the nature of the propagation of sound is elastic compression or deformation, the vacuum cannot transmit sound.

The minimal model for the sound wave transmission is the discrete 1D mass-spring model, where the infinistesimal local purturbation will propagate through the one dimensional chain by the wave speed $v=\sqrt{k/m}$ ($k$ is the spring stiffness and $m$ is the mass). Althrough it is a oversimplified model, this methodology is widely used in solid state physics. The message we can extract from this model, is that, the speed of sound is higher in stiff material and lower in material with higher density. Intuitively, the bulk modulus of liquids is ~GPa level, and for gas is ~MPa level, where the density of liquid fails to upscale. Therefore, sound normally propagates faster in liquids.

Based on the methodology of this oversimplified model, we can actually derive the speed of sound based on continuum mechanics. The derivation can be done in two ways: one is based on the straightforward "engineering" ways, using the concept of the infinitesimal volume of continuum, and the second is more rigorous, starting from the governing equation and using linear purturbation theory to derive a linearized wave equation. In the following, I will introduce both ways in details.

## "Physical" derivation

Consider a static medium (can be gas, liquids or solids) under a small one dimensional purturbation. A practical example is using a piston to slightly compress air in a thin tube. We call the purturbed air as waveback and the unpurburbed air as wavefront. Assume the pressure, density and velocity (under the lab reference frame) of the waveback is $p+\mathrm{d}p$, $\rho+\mathrm{d}\rho$ and $\mathrm{d}v$, and for wavefront is $p$, $\rho$ and $0$. Next, assume the proprgation of the purturbation is $c$, and we change the reference frame to a hypothetical one with the relative velocity $\mathbf{c}$ against the lab reference frame. As $c$ is a constant, the hypothetical reference frame is still inertial, and we can write the velocity of waveback and wavefront as $-c+\mathrm{d}v$ and $-c$.

Why we are bothering do this transformation? First, we need to explicitly introduce $c$ into the conservation laws. Second, this transformation garuantees steady condition for all physical variables, namely, $\frac{\partial p}{\partial t}=\frac{\partial \rho}{\partial t}=0$. List the conservation of mass and momentum for the infinitesimal volume under the hypothetical reference frame, we have:

$$ (-c+\mathrm{d}v)(\rho+\mathrm{d}\rho) = -c\rho, \label{eq1}\tag1 $$

$$ (-c+\mathrm{d}v)^2(\rho+\mathrm{d}\rho) + (p+\mathrm{dp}) = c^2\rho + p, \label{eq2}\tag2 $$

where the first equation gives $c\mathrm{d}\rho=\rho\mathrm{d}v$, and the second gives $c\rho\mathrm{d}v=\mathrm{d}p$, and it follows $c = \sqrt{\mathrm{d}p/\mathrm{d}\rho}$. This is exactly the expression for the speed of sound, and this equation is compariable with the discrete version of wave speed $v=\sqrt{k/m}$. Normally, $\mathrm{d}p/\mathrm{d}\rho$ is a material property given by thermodynamic equation of states.

It should be noted that the above derivation is heruistic due to its oversimplification (one dimension, constant wave speed, steady state).

## "Mathematical" derivation

A more rigorous method is using linear purturbation theory of the governing equations. Here we use ideal liquids as an example, which can be described by the Euler equation and the mass conservation equation:

$$ \frac{\partial \mathbf{v}}{\partial t} + \mathbf{v}\cdot\nabla\mathbf{v} + \frac{1}{\rho}\nabla p = 0,  \label{eq3}\tag3 $$

$$ \frac{\partial \rho}{\partial t} + \nabla\cdot(\rho\mathbf{v}) = 0. \label{eq4}\tag4 $$

Let's see if we can linearize the Equation (\ref{eq3}) and (\ref{eq4}) to find a classical wave equation. Assume linear purturbation $\rho(\mathbf{x}, t) = \rho_0 + \rho_{\epsilon}(\mathbf{x}, t)$, $p(\mathbf{x}, t) = p_0 + p_{\epsilon}(\mathbf{x}, t)$, and $v(\mathbf{x}, t) = v_{\epsilon}(\mathbf{x}, t) = O(\epsilon)$, where $\epsilon << 1$. Linearizing the Equation (\ref{eq3}) and (\ref{eq4}) gives:

$$ \frac{\partial \rho_{\epsilon}}{\partial t} + \rho_0\nabla\cdot\mathbf{v} = 0, \label{eq5}\tag5 $$

$$ \rho_0\frac{\partial \mathbf{v}}{\partial t} = -\frac{\mathrm{d}p}{\mathrm{d}\rho}\nabla\rho_{\epsilon}. \label{eq6}\tag6 $$

It is obvious to get a classical wave equation from the Equation (\ref{eq5}) and (\ref{eq6}):

$$ \frac{\partial^2 \rho_{\epsilon}}{\partial t^2} = \frac{\mathrm{d}p}{\mathrm{d}\rho}\nabla^2 \rho_{\epsilon}, \label{eq7}\tag7 $$

and the wave speed $c$ is given by $\sqrt{\mathrm{d}p/\mathrm{d}\rho}$. This linear purturbation method is important and very common in the analysis of dynamical systems.