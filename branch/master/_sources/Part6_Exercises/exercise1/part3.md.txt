# Part 3: Parallel execution on compute nodes

This page covers running parallel code on compute nodes using the job submission system.

## Compile the parallel version of the code

```
    cd C-OMP
    make
```
Output:
```
    cc -fopenmp -g -c sharpen.c
    cc -fopenmp -g -c dosharpen.c
    cc -fopenmp -g -c filter.c
    cc -fopenmp -g -c cio.c
    cc -fopenmp -g -c utilities.c
    cc -fopenmp -g -o sharpen sharpen.o dosharpen.o filter.o cio.o utilities.o -lm
```

To run this code in parallel it should be submitted to the compute nodes using Slurm workload manager.

## Running on the {{ machine_name }} compute nodes

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise1/ex1_slurm.md\n```'.replace("REPLACE",machine_name) }}


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

## Things to think about ...

Before we move on a couple of things to consider:

- Can you explain why the x axis on the plot of parallel speedup only extend to 128 cores on Archer2?
- Using the code provided for the threaded version of sharpen are you able to reproduce the plot given for Archer2 on your own system? How do the results compare?
- There are other version of the sharpen example that employ different methods to parallelise the calculation. One uses message passing and the other is a hybrid that combines message passing and shared memory parallism.  additionally try to recreate the plot on speedup for using these codes. How does the data compare?

