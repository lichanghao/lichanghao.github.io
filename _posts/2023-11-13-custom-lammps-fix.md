---
title:             "Coding custom LAMMPS fix command"
date:              2023-11-13
permalink:         /posts/2023/11/13/custom-lammps-fix
tags:              Note
---

This post is a note on coding custom fix command in LAMMPS.

## LAMMPS fix command

According to the [official documentation](https://docs.lammps.org/Manual.html) and the [LAMMPS programmer's guide](https://docs.lammps.org/Developer.html), the fix command classes are designed for probing and modifying the computation during (but not limited to) the time integration. The fix style is very versatile and powerful, as it can almost be used to implement anything custom in LAMMPS.

## Name your custom fix

I have written several in-house LAMMPS fix command by myself. Due to the great extensibility of the LAMMPS code design, the programmer can just implement his own fix command by only one pair of cpp and header files. 

The first thing for a new fix command is naming the cpp, header, and the class. For example, the implementation of NVE time integration is contained in `fix_nve.cpp` and `fix_nve.h`, where the class name is `FixNVE`. It should be noted that the developer needs to use a special marco to register the name of this fix to the LAMMPS main program. An example is:

```cpp
#ifdef FIX_CLASS
// clang-format off
FixStyle(nve,FixNVE);
// clang-format on
#else
```

In the marco function `FixStyle()`, the first argument is the name of this fix, such that the corresponding command in the input file will be `fix nve ...`. The second argument is the name of the C++ class in your code.

## Define the basic behavior of your fix

Fix commands are powerful because they can be invoked in almost every mini-step of the LAMMPS time integration, and they support creating different kinds of data structures and have access to all information that LAMMPS creates. Follow the examples in [Steve's talk slides](https://www.lammps.org/tutorials/italy14/italy_modify_Mar14.pdf) to get more hints.

The behavior of the fix can be tuned by setting the inherited flags in `fix.h`, which control the data structure, output, and parallel behavior of your own fix. Also, several bit-wise masks can be invoked to tell the LAMMPS main program, which define how the fix should take effect during different stages of the time integration. See the following examples:


```cpp
// in the constructor of your custom fix class
  dynamic_group_allow = 1;
  peratom_flag = 1;
  size_peratom_cols = 0;
  peratom_freq = 1;
  time_integrate = 1;
  create_attribute = 1;
```

```C++
int FixNVE::setmask()
{
  // define fix behavior during the timestep
  int mask = 0;
  mask |= INITIAL_INTEGRATE;
  mask |= FINAL_INTEGRATE;
  return mask;
}
```

See the source code comments of `fix.h` or the [LAMMPS Programmer Guide](https://docs.lammps.org/Developer.html) for more details.

## Create and refer per-atom array

Fix commands support creating user-defined data structure associated to each atom or each local processor. For the array (vector or tensor) of C++ primitive types (int, float, etc.), use the pre-defined utility function for allocating memory:

```cpp
// add this two line to the class constructor
  FixFiberSliding::grow_arrays(atom->nmax); // overrided function allocating per-atom array
  atom->add_callback(Atom::GROW); // register the behavior of this fix to the atom class
```

The definition of `grow_arrays()`:

```cpp
void FixFiberSliding::grow_arrays(int nmax)
{
  memory->grow(atom_dislocation_label, nmax, "fiber/sliding:atom_dislocation_label");
  vector_atom = atom_dislocation_label;
}
```

Also, the programmer needs to override `memory_usage()`, `copy_arrays()`, `pack_exchange()` and `unpack_exchange()` to ensure the correct behavior of parallel computing. LAMMPS will automatically invoke those functions, but the programmer needs to make sure the overrided logic is correct. See the following examples:

```cpp
/* ----------------------------------------------------------------------
   memory usage of local atom-based array
------------------------------------------------------------------------- */

double FixFiberSliding::memory_usage()
{
  double bytes = (double) atom->nmax * 1 * sizeof(double);
  return bytes;
}

/* ----------------------------------------------------------------------
   allocate atom-based array
------------------------------------------------------------------------- */

void FixFiberSliding::grow_arrays(int nmax)
{
  memory->grow(atom_dislocation_label, nmax, "fiber/sliding:atom_dislocation_label");
  vector_atom = atom_dislocation_label;
}

/* ----------------------------------------------------------------------
   copy values within local atom-based array
------------------------------------------------------------------------- */

void FixFiberSliding::copy_arrays(int i, int j, int /*delflag*/)
{
  atom_dislocation_label[j] = atom_dislocation_label[i];
}

/* ----------------------------------------------------------------------
   initialize one atom's array values, called when atom is created
------------------------------------------------------------------------- */

void FixFiberSliding::set_arrays(int i)
{
  atom_dislocation_label[i] = 0;
}

/* ----------------------------------------------------------------------
   pack values in local atom-based array for exchange with another proc
------------------------------------------------------------------------- */

int FixFiberSliding::pack_exchange(int i, double *buf)
{
  int n = 0;
  buf[n++] = atom_dislocation_label[i];
  return n;
}

/* ----------------------------------------------------------------------
   unpack values in local atom-based array from exchange with another proc
------------------------------------------------------------------------- */

int FixFiberSliding::unpack_exchange(int nlocal, double *buf)
{
  int n = 0;
  atom_dislocation_label[nlocal] = buf[n++];
  return n;
}
```

Note that ```set_arrays()``` is not mandatory. As long as the memory communicating logic is correct, LAMMPS will automatically take care of the rest part of the parallel algorithm, and an array with the name of the fix ID can be directly used in the input script for output:

```bash
// this fix defines a scalar on each atom, which can be refered as f_1
fix 1 fiber/sliding ...
```