---
layout:            post
title:             "FEniCSx study notes"
date:              2022-02-05 16:27:00 +0800
tags:              Research
comments:			  true
category:          Features
catalog:    		  true
header-img: 		  "img/post-bg-e2e-ux.jpg"
header-mask:       0.3
author:            Changhao Li
---

This note records my study route of FEniCS, an open-source finite element library.

## About FEniCS

FEniCS is an acronym with FE representing finite element, CS representing computational software. The FEniCS software package was originally compiled at the University of Chicago, whose mascot is a phoenix, which likely inspired the name. 

Beginning from 2018, the FEniCS project adapted a new framework, including multiple building blocks: UFL, Basix, FFCx and DOLFINx. The legacy version of FEniCS is composed of UFL, FIAT, FFC and DOLFIN. There are multiple versions of documentations/tutorials online, for both newest release and the legacy version. Also, the questions/answers online are based on different versions. **The grammar for FEniCSx and its legacy version is quite different. Newest version is recommended.**

## Useful resources


1. [FEniCSx Discourse Group](https://fenicsproject.discourse.group)

2. [FEniCSx Tutorial (New version, not very complete, mainly for dolfinx)](https://jsdokken.com/dolfinx-tutorial/fem.html)

3. [FEniCS Book (Based on lagacy version, not even 2019.1)](http://launchpad.net/fenics-book/trunk/final/+download/fenics-book-2011-10-27-final.pdf)

4. [FEniCSx Github (Newest version)](https://github.com/FEniCS)

5. [FEniCSx official website](https://fenicsproject.org)

## Get started: 1D boundary value problem

