---
title:             "Poisson-Nernst-Planck-system"
date:              2023-08-10
permalink:         /posts/2023/08/10/Nernst-Planck-system
tags:              Research
---

This post is a brief introduction to Poisson-Nernst-Planck equations.

## Poisson-Nernst-Planck System

In batteries, fuel cells, and other natual/artificial electrochemical systems, the coupled diffusion effect and electro-migration effect of the charged particles can be modeled by coupling Nernst-Planck equation and Poisson equation. The Nernst-Planck equation can be written as

$$ \frac{\partial c_i}{\partial t} = \nabla \cdot \left\{ D_i\left[\nabla c_i + {z_ie\over{k_\text{B}T}}c_i \left( \nabla \phi + {\partial {\bf A}\over{\partial t}} \right) \right] \right\} \label{eq1}\tag1 ,$$

where $c_i$ is the concentration of the $i^{th}$ charged particle, $D_i$, $z_i$ is the corresponding diffusivity and ion valence, $\phi$ is the electrostatic potential, $\mathbf{A}$ is the magnetic field. It should be note that $\frac{e}{k_{\rm{B}}T} = \frac{F}{R T}$, the same equation in microscopic/marcoscopic views. Also, $\mathbf{A}$ can be neglected given the absence of magnetic field.

The Poisson equation is just taken from the Maxwell equations:

$$ \nabla\cdot(\epsilon \nabla\phi) = \rho, \label{eq2}\tag2 $$

where $\epsilon$ is the permitivity and $\rho = \sum_{i}z_iec_i$ is the charge density. Now let's see if the coupled system is closed. There are totally $i+1$ scalar fields to be solved, namely $c_i$ and $\phi$, and there are corresponding $i+1$ linearly independent PDEs. Intuitively, given suitable initial/boundary conditions, the problem is well-posed.

## Simplification in different time scales

In conductors, the free charge can rapidly migrate to reach electrostatic balance ($\frac{\partial \rho}{\partial t}=0$ everywhere), just like the diffusion balance of particles. The characteristic time scale of electrostatic balance ($\tau_{\rm{elec}} \sim \frac{\epsilon}{\sigma}$) is much greater than the time scale of diffusive balance ($\tau_{\rm{diff}} \sim \frac{L}{D}$). The mismatch of time scales enables us to directly assume electrostatic balance without solving the Poisson-Nernst-Planck equation (\ref{eq1}) in $\tau_{\rm{elec}}$ time scale, given $\tau_{\rm{diff}} >> \tau_{\rm{elec}}$.

Normally this simplification can be applied in the case where we focus more on the "slow" diffusion-migration process of charged particle in the device level. For example, consider what happens in the liquid electrolyte of a simple electrolyzer containing $H^+$ and $Cl^-$, we have

$$ \frac{\partial c_1}{\partial t} = \nabla \cdot \left\{ D_1\left[\nabla c_1 + {z_1e\over{k_\text{B}T}}c_1 \left( \nabla \phi \right) \right] \right\} \label{eq3}\tag3 ,$$

$$ \frac{\partial c_2}{\partial t} = \nabla \cdot \left\{ D_2\left[\nabla c_2 + {z_2e\over{k_\text{B}T}}c_2 \left( \nabla \phi \right) \right] \right\} \label{eq4}\tag4 ,$$

$$ -\nabla\cdot(\epsilon \nabla\phi) = z_1ec_1+z_2ec_2. \label{eq5}\tag5 $$

Note that the Equation (\ref{eq5}) is often replaced by

$$ \nabla\cdot(\sigma \nabla\phi + z_1eD_1\nabla c_1 + z_2eD_2\nabla c_2) = 0, \label{eq6}\tag6 $$

where $\sigma = \sum_i\frac{z_iec_i}{k_{\rm{B}}T}$ is the total electronic conductivity. This equation gives local current balance. Note that, combined with Equation (\ref{eq3}-\ref{eq4}), Equation (\ref{eq6}) is simply $z_1e\frac{\partial c_1}{\partial t} + z_2e\frac{\partial c_2}{\partial t} = 0$.

Actually Equation (\ref{eq5}) and Equation (\ref{eq6}) is not mathematically compatible. In most of literature, Equation (\ref{eq3}-\ref{eq4}) is coupled with Equation (\ref{eq5}) if the model focuses on $\tau_{\rm{elec}}$, while coupled Equation (\ref{eq6}) for $\tau_{\rm{diff}}$. By intuition, there should not have any incompatible equations and any timescale should be included in Maxwell equations.

## Subtle differences between two simplifications

Let's go back to the classical derivation of electrostatic balance. Consider the following problem where the free charge flows in a conductor:

$$ -\nabla\cdot(\epsilon\nabla\phi) = \rho, \label{eq7}\tag7 $$

$$ \nabla\cdot(\sigma\nabla\phi) = \frac{\partial\rho}{\partial t}. \label{eq8}\tag8 $$

We can easily regroup the equation sets into:

$$ \frac{\partial \rho}{\partial t} = -\frac{\sigma}{\epsilon}\rho, \label{eq9}\tag9 $$

and then, the general solution for Equation (\ref{eq8}) is $\rho(\mathbf{x}, t) = \exp(-\frac{\epsilon}{\sigma}t)f(\mathbf{x})$, where $f$ is only the function of space. This general solution indicates, the dynamics of the free charge density is exponential decay, with the timescale $\tau_{\rm{elec}} = \frac{\epsilon}{\sigma}$. Again, in the device level of simulations, we focus on longer time scale $\tau_{\rm{diff}} >> \tau_{\rm{elec}}$. And in this limit, we have the steady constraints $\rho = z_1ec_1 + z_2ec_2 = 0$, and $\frac{\partial \rho}{\partial t} = 0$. Substitute the steady condition into Equation (\ref{eq5}) and Equation (\ref{eq6}), we get

$$ -\nabla\cdot(\epsilon\nabla\phi) = 0, \label{eq10}\tag{10} $$

$$ z_1e\frac{\partial c_1}{\partial t} + z_2e\frac{\partial c_2}{\partial t} = 0. \label{eq11}\tag{11} $$

If we close observe Equation (\ref{eq10}) and Equation (\ref{eq11}), we can see the subtlety that they are redundant in the most general cases. Equation (\ref{eq11}) is originated from the steady state assumption, and Equation (\ref{eq10}) or the Poisson equation actually governs the dynamics of the electrostatic potential. Then why we have this compatibility? This is because the oversimplification - both the permitivity $\epsilon$ and the conductivity $\sigma$ are functions of the concentrations $c_1$ and $c_2$.

Therefore, the strict PDEs describing the migration-diffusion effects in solutions should be:

$$ \frac{\partial c_1}{\partial t} = \nabla \cdot \left\{ D_1\left[\nabla c_1 + {z_1e\over{k_\text{B}T}}c_1 \left( \nabla \phi \right) \right] \right\} \label{eq12}\tag{12} ,$$

$$ \frac{\partial c_2}{\partial t} = \nabla \cdot \left\{ D_2\left[\nabla c_2 + {z_2e\over{k_\text{B}T}}c_2 \left( \nabla \phi \right) \right] \right\} \label{eq13}\tag{13} ,$$

$$ -\nabla\cdot(\epsilon(c_1, c_2) \nabla\phi) = z_1ec_1+z_2ec_2. \label{eq14}\tag{14} $$

This PDE set can be applied to all timescale. Considering the real computational cost solving the equations, we can assume: (1) $\rho = z_1ec_1 + z_2ec_2 = 0$ if we don't care the balance process of free charges. (2) $\frac{\partial c_1}{\partial t} = \frac{\partial c_2}{\partial t} = 0$ if we don't care the diffusion process.