---
title:             "Compile VASP on Apple M1 Max CPU"
date:              2023-11-15
permalink:         /posts/2023/11/13/compile-VASP-on-Apple-M1
tags:              VASP
---

This post records the compilation of VASP on my laptop.

## VASP package

The [Vienna Ab initio Simulation Package](https://www.vasp.at) (VASP) is the most famous and popular density functional theory (DFT) package in the world. Refer to [VASP wiki](https://www.vasp.at/wiki/index.php/The_VASP_Manual) and the (highly cited) papers written by the developer teams for the theoretical background and more details.

## VASP compatibility

The VASP source code is distributed under commercial license. Normally, the code is recommended to be deployed on high performance clusters (HPC) as research-level DFT calculation normally needs large amount of computational resources far beyond personal computers. The developer team only provides official installation instruction on Linux. However, it is a good practice to configure and compile VASP on your personal computer (for study and for dun), especially MacOS system.

## Install VASP 6.1.0 on MacOS Sonoma, Apple M1 Max CPU

First you need download the source code. I cannot upload the code here because it would be a violation of the VASP license.

The [official guide for install VASP 6.x.x](https://www.vasp.at/wiki/index.php/Installing_VASP.6.X.X) can be found on the VASP wiki. Note that it is for Linux. On MacOS, you need to manually configure dependencies and modify several places of the makefile and code. Note that, the VASP 5.x.x is not compatible with the following method.

Here goes some critical step you need to do, to successfully compile and run VASP on the MacOS system:

Install dependencies.

```zsh
brew install gcc openmpi scalapack fftw qd openblas hdf5
```

Modify the `makefile.include`. Here is my final version of the makefile. Note that you need to manually modify several places for your compiler (I am using `gcc-11.4`), and the absolute path of `BLAS`, `LAPACK`, `FFTW` and `hdf5`. No need to touch `makefile`. Follow [this blog](https://gist.github.com/janosh/a484f3842b600b60cd575440e99455c0) for more details.

```makefile
# Default precompiler options
CPP_OPTIONS = -DHOST=\"LinuxGNU\" \
              -DMPI -DMPI_BLOCK=8000 -Duse_collective \
              -DscaLAPACK \
              -DCACHE_SIZE=4000 \
              -Davoidalloc \
              -Dvasp6 \
              -Duse_bse_te \
              -Dtbdyn \
              -Dfock_dblbuf \
              -D_OPENMP \
              -Dqd_emulate

CPP         = gcc-11 -E -C -w $*$(FUFFIX) >$*$(SUFFIX) $(CPP_OPTIONS)

FC          = mpif90 -fopenmp
FCL         = mpif90 -fopenmp

FREE        = -ffree-form -ffree-line-length-none

FFLAGS      = -w -ffpe-summary=invalid,zero,overflow -L /opt/homebrew/Cellar/gcc@11/11.4.0/lib/gcc/11
FFLAGS      += -march=native

OFLAG       = -O2
OFLAG_IN    = $(OFLAG)
DEBUG       = -O0

OBJECTS     = fftmpiw.o fftmpi_map.o fftw3d.o fft3dlib.o
OBJECTS_O1 += fftw3d.o fftmpi.o fftmpiw.o
OBJECTS_O2 += fft3dlib.o

# For what used to be vasp.5.lib
CPP_LIB     = $(CPP)
FC_LIB      = $(FC)
CC_LIB      = gcc-11
CFLAGS_LIB  = -O
FFLAGS_LIB  = -O1
FREE_LIB    = $(FREE)

OBJECTS_LIB = linpack_double.o

# For the parser library
CXX_PARS    = g++-11
LLIBS       = -lstdc++
QD         ?= /opt/homebrew
LLIBS      += -L$(QD)/lib -lqdmod -lqd
INCS       += -I$(QD)/include/qd

##
## Customize as of this point! Of course you may change the preceding
## part of this file as well if you like, but it should rarely be
## necessary ...
##

# When compiling on the target machine itself, change this to the
# relevant target when cross-compiling for another architecture
FFLAGS     += -march=native

# For gcc-10 and higher (comment out for older versions)
FFLAGS     += -fallow-argument-mismatch

# BLAS and LAPACK (mandatory)
OPENBLAS_ROOT ?= /opt/homebrew/Cellar/openblas/0.3.24
BLASPACK    = -L$(OPENBLAS_ROOT)/lib -lopenblas

# scaLAPACK (mandatory)
SCALAPACK_ROOT ?= /opt/homebrew
SCALAPACK   = -L$(SCALAPACK_ROOT)/lib -lscalapack

LLIBS      += $(SCALAPACK) $(BLASPACK)

# FFTW (mandatory)
FFTW_ROOT  ?= /opt/homebrew
LLIBS      += -L$(FFTW_ROOT)/lib -lfftw3 -lfftw3_omp
INCS       += -I$(FFTW_ROOT)/include

# HDF5-support (optional but strongly recommended)
CPP_OPTIONS+= -DVASP_HDF5
HDF5_ROOT  ?= /opt/homebrew/Cellar/hdf5/1.14.3
LLIBS      += -L$(HDF5_ROOT)/lib -lhdf5_fortran
INCS       += -I$(HDF5_ROOT)/include

# For the VASP-2-Wannier90 interface (optional)
#CPP_OPTIONS    += -DVASP2WANNIER90
#WANNIER90_ROOT ?= /path/to/your/wannier90/installation
#LLIBS          += -L$(WANNIER90_ROOT)/lib -lwannier

# For the fftlib library (experimental)
#CPP_OPTIONS+= -Dsysv
#FCL        += fftlib.o
#CXX_FFTLIB  = g++-11 -fopenmp -std=c++11 -DFFTLIB_THREADSAFE
#INCS_FFTLIB = -I./include -I$(FFTW_ROOT)/include
#LIBS       += fftlib
#LLIBS      += -ldl
```

Further change `src/parser/makefile`. Change the line 16 to `ar vq libparser.a $(CPPOBJ_PARS) $(COBJ_PARS)`. Otherwise, the compilation cannot pass.

Now the compilation should be okay, which means you have executables as `bin/vasp-std`. However, successful compilation does not guarantee 100% correctness. You need to follow [this discussion](https://www.vasp.at/forum/viewtopic.php?t=17831) to modify few lines of `src/reader_base.F`, to make sure VASP can deal with the input file correctly. This essentially means you need to change the original definition of the variable `flag_value`

```c++
intent(in)  :: flag_name
```

to

```c++
intent(inout)  :: flag_name
```

Note that VASP 6.1.0 test suites cannot be compiled on MacOS due to bugs. See the [notes of developer](https://www.vasp.at/info/post/bugfix-in-testsuite-vasp6/).

Install and configure `atomate2`, which is a manager for high-throughput simulations. Follow the [official guide of installation](https://materialsproject.github.io/atomate2/user/install.html) to link `atomate2` with VASP. The only thing you need to notice is that, the URI provided from MangoDB is slightly different from the format that `atomate2` needs. You need to append the name of your database in the middle, following this format:

```html
mongodb+srv://<<USERNAME>>:<<PASSWORD>>@<<HOST>>/<<DB_NAME>>?retryWrites=true&w=majority
```

## Play around with atomate2 and VASP

Example for grid-search test for the different configuration of `num_procs`, `OMP_NUM_THREADS`, and `NCORES`:

```python
"""This script grid-searches OMP_NUM_THREADS, NCORE and number of MPI processes for
minimal VASP runtime on a simple Si2 relaxation. It writes the results to CSV and copies
markdown table to clipboard. Requires Python 3.10. Invoke with
python vasp-perf-grid-search.py 2>&1 | tee Si-relax.log
to keep a log.
"""

import os
import warnings
from itertools import product
from time import perf_counter, sleep

import pandas as pd
from atomate2.vasp.jobs.core import RelaxMaker
from atomate2.vasp.powerups import update_user_incar_settings
from jobflow import run_locally
from pandas.io.clipboard import clipboard_set
from pymatgen.core import Structure


warnings.filterwarnings("ignore")  # suppress pymatgen warnings clogging up the logs

VASP_BIN = "/Users/janosh/dev/vasp/compiled/vasp_std_6.3.0_m1"
results: list[tuple[int, int, int, float]] = []

# construct an FCC silicon structure
si_structure = Structure(
    lattice=[[0, 2.73, 2.73], [2.73, 0, 2.73], [2.73, 2.73, 0]],
    species=["Si", "Si"],
    coords=[[0, 0, 0], [0.25, 0.25, 0.25]],
)

# grid-search OMP_NUM_THREADS, NCORE and number of MPI processes
try:
    prod = list(product([1, 2, 4, 8], [1, 2], [2, 4]))
    for idx, (n_proc, n_threads, n_core) in enumerate(prod, 1):

        os.environ["OMP_NUM_THREADS"] = str(n_threads)

        print(f"Run {idx} / {len(prod)}")

        # make a relax job to optimize the structure
        relax_job = RelaxMaker(
            run_vasp_kwargs={"vasp_cmd": f"mpiexec -np {n_proc} {VASP_BIN}"}
        ).make(si_structure)

        relax_job = update_user_incar_settings(relax_job, {"NCORE": n_core})

        start = perf_counter()
        # run the job
        run_locally(relax_job, create_folders=True, ensure_success=True)

        elapsed = perf_counter() - start
        print(
            f"Si relaxation with {n_proc=}, {n_threads=}, {n_core=} took "
            f"{elapsed:.1f} sec"
        )
        results.append((n_proc, n_threads, n_core, elapsed))

        print("Waiting 10 secs to cooldown...\n\n", flush=True)
        sleep(10)  # so every run is a bit more like the first


except KeyboardInterrupt:  # exit gracefully on ctrl+c and write partial results
    print("Job was interrupted")


df = pd.DataFrame(results, columns=["n_proc", "n_threads", "n_core", "elapsed"])
df.round(2).to_csv("vasp-perf-results.csv")
clipboard_set(df.to_markdown())
```

When you are not explicitly setting `OMP_NUM_THREADS`, don't forget to manually set it to 1, to prevent possible unexpected behaviors.

```zsh
export OMP_NUM_THREADS=1
```