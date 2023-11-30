---
title:             "Compile and run MOOSE on HPC"
date:              2023-11-30
permalink:         /posts/2023/11/30/compile-and-run-moose-on-hpc
tags:              Moose
---

This post is a note on installing and running the finite element framework MOOSE on HPC.

## Set up installing environment

Based on the [official instruction](https://mooseframework.inl.gov/getting_started/installation/hpc_install_moose.html), the minimum requirement to compile MOOSE is:

1. GCC/Clang C++17 compliant compiler (GCC @ 7.5.0, Clang @ 10.0.1 or greater, Intel compilers are not supported).
2. CMake.
3. Python 3.7 or greater version.

The above requirement can be normally satisfied by using pre-installed compiling environments on HPC. The following examples are tested on the UCSD Expanse cluster.

```bash
module load cpu
module load gcc/10.2.0
module load mvapich2/2.3.7
module load cmake/4.1.2
module load python/3.8.1
```

MPI compilers can be used to speed up the compilation. First checking the environment variable to see if the MPI compiler path is correctly configured:

```bash
echo $CC $CXX $FC $F90 $F77
```

If nothing shows or the returned variable is not something like `mpicc` but `gcc`, manually set up the environmental variables by

```bash
export CC=mpicc CXX=mpicxx FC=mpif90 F90=mpif90 F77=mpif77
```

## Clone MOOSE

```bash
mkdir -p ~/projects
cd ~/projects
git clone https://github.com/idaholab/moose.git
cd moose
git checkout master
```

## Build prerequisite libraries

MOOSE depends on `PETSc`, `Libmesh` and `WASP`. 

```bash
cd ~/projects/moose/scripts
export MOOSE_JOBS=6 METHODS=opt
./update_and_rebuild_petsc.sh
./update_and_rebuild_libmesh.sh
./update_and_rebuild_wasp.sh
```

## Compile and test MOOSE

Now finally MOOSE is ready to compile.

```bash
cd ~/projects/moose/test
make -j 6
```

After the successful compilation, run the following command to test:

```bash
cd ~/projects/moose/test
./run_tests -j 6
```

Note that the test result depends on the environment of your HPC. Some HPC using `slurm` does not allow explicit call of `mpirun`, and this will cause test failure. But as long as the compilation is successful and all dynamic linked library is ready to use, you can directly compile you own program and submit it by `slurm` scripts.

## Submit slurm jobs

Here is an example script:

```bash
#!/bin/sh
#SBATCH --job-name=moose                 #Job name
#SBATCH --nodes=1                        #Number of nodes (servers, 32 proc/node)
#SBATCH --ntasks=16                      #Number of tasks/MPI RankS
#SBATCH --ntasks-per-node=16             #Tasks per node
#SBATCH --ntasks-per-socket=8            #Tasks per socket
#SBATCH --cpus-per-task=1                #Number of CPU per task
#SBATCH --mem-per-cpu=3600mb             #Memory (120 gig/server)
#SBATCH --distribution=cyclic:cyclic     #Distribute tasks cyclically 
#SBATCH --time=12:00:00                  #Walltime days-hh:mm:ss
#SBATCH --output=moose-%j.out            #Output and error log
#SBATCH --mail-type=END,FAIL             #When to email user
#SBATCH --mail-user=your-email@ufl.edu   #Email address to send mail to
#SBATCH --account=group_name             #Allocation group name,
#SBATCH --qos=group_name                 #Add -b for burst job

srun --mpi=pmix_v3 ~/projects/moose/modules/combined-opt -i moose_input_file.i
```