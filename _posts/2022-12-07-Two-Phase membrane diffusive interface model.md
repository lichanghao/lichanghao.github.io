---
layout:            post
title:             "Two-Phase Diffusive Interface Model of Biological Membranes"
date:              2022-12-07 19:27:00 +0800
tags:              Research
comments:			  true
category:          Features
catalog:    		  true
header-img: 		  "img/post-bg-e2e-ux.jpg"
header-mask:       0.3
author:            Changhao Li
---

This is the development log for a project I was recently working on.

## Formalism

Consider an closed elastic membrane, whose total Helmholtz free energy can be written as:

$$E_{el}=\int_{A}\frac{k}{2}(\kappa-c_0)^2dA \label{eq1}\tag1$$

where $k$ is the bending modulus, $\kappa(\mathbf{r})$ is the local curvature, and $c_0$ is the spontaneous curvature. Note that, in Eq($\ref{eq1}$) we neglect the contribution of surface energy and gaussian curvature.

As stated in [2008 Qiang Du et al.](https://link.springer.com/article/10.1007/s00285-007-0118-2), the above energy can be reformulated in a way of phase-field modeling, by

$$ E_{el}(\phi) = \int_{\Omega} \frac{k}{2\epsilon}(\epsilon\Delta\phi+(\frac{1}{\epsilon}\phi+c_0\sqrt{2})(1-\phi^2))^2dV \label{eq2}\tag2$$

where $c_0$ is the spontaneous curvature, $\phi$ (ranging from $-1$ to $1$) is the order parameter indicating the membrane shape, and $\epsilon$ is the parameter controling the gradient thickness for $\phi$. The integral is performed on the whole 3D domain $\Omega$, which avoids explicitly tracking the moving interface.

The Equation (\ref{eq2}) is the continuum version of the membrane elastic bending energy. To model the two-phase membrane, we need to introduce one additional order parameter $\eta$, ranging from $-1$ to $1$ also, to indicate the phase separation on the membrane. Since the phase boundary normally introduces extra elastic energy, here we define the line tension (interfacial energy) between two phases ($\eta = -1$ and $\eta = 1$) as:

$$ L = \int_{l} \gamma_0 dl \label{eq3}\tag3 $$

which is simply the line integral of the interfacial energy density $\gamma_0$. Similarly, the continuum version of Equation (\ref{eq3}) is:

$$ L(\phi, \eta) = \int_{\Omega}\delta [\frac{\epsilon}{2}|\nabla\phi|^2 + \frac{1}{4\epsilon}(\phi^2-1)^2][\frac{\xi}{2}|\nabla\eta|^2 + \frac{1}{4\xi}(\eta^2-1)^2]d\Omega \label{eq4}\tag4$$

where $\delta$ is the magnitude of line tension, $\epsilon$ and $\xi$ are   parameters controlling the interfacial thickness of $\phi$ and $\eta$, respectively. Apparently, the energy density of $L(\phi, \eta)$ only take non-zero value near the phase bounaries, which is exactly what we want, to automatically tracking the moving bounadries again.

Since now we have two phases ($\eta=1$ and $\eta=0$) on the membrane denoted by the order parameter $\phi$, the Equation (\ref{eq2}) should also be the function of $\eta$. It is easy to come up with, $\eta$ changes the physical properties of local regions. Here we assume the bending modulus $k$ and the spontaneous curvature $c_0$. The modified bending energy function is:

$$ E_{el}(\phi, \eta) = \int_{\Omega} \frac{k(\eta)}{2\epsilon}(\epsilon\Delta\phi+(\frac{1}{\epsilon}\phi+c_0(\eta)\sqrt{2})(1-\phi^2))^2dV \label{eq5}\tag5$$


where $k(\eta)$ and $c_0(\eta)$ give constitutive relation for different phases. The two parameter create freedom for a variety of different physical problems.

In real world, membrane is easy to bend but difficult to stretch. Also, closed biomembrane always keep nearly constant volume becaue of osmotic pressure. We further add two energy penalty term to model the constaint on surface area and the total volume, written as:


