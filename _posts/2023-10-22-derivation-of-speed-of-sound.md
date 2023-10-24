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

Consider a static medium (can be gas, liquids or solids) under a small