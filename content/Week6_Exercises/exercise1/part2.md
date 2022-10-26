# Part 3: Parallel execution on compute nodes

This page covers running parallel code on compute nodes using the job submission system.


## Compile the parallel version of the code

```
    cd C-OMP
    make
```
Output:
```
    cc -fopenmp -g -DC_OPENMP_PRACTICAL -c sharpen.c
    cc -fopenmp -g -DC_OPENMP_PRACTICAL -c dosharpen.c
    cc -fopenmp -g -DC_OPENMP_PRACTICAL -c filter.c
    cc -fopenmp -g -DC_OPENMP_PRACTICAL -c cio.c
    cc -fopenmp -g -DC_OPENMP_PRACTICAL -c utilities.c
    cc -fopenmp -g -DC_OPENMP_PRACTICAL -o sharpen sharpen.o dosharpen.o filter.o cio.o utilities.o -lm
```

To run this code in parallel it should be submitted to the compute nodes using Slurm workload manager.

## Running on the {{ machine_name }} compute nodes

{{  '```{include} ../../substitutions/substitutions_REPLACE/ex1_slurm.md\n```'.replace("REPLACE",machine_name) }}


## Investigating the parallel speedup

You will notice that two timings are reported: the calculation time, and the overall runtime. The first excludes the file input/output operations.

The speedup is calculated by diving the the time taken to run on one core by the time taken to run using N cores. For this program you can calculate the speedup for both the calculation time and the overall runtime

Example speedup results for ARCHER2 are shown below.

```{figure} ./images/sharpen_speedup.svg
---
class: with-border
alt: speedup graph
---

Speedup results for ARCHER2
```
