# Part 2: Weak Scaling

In this section you will use a different benchmark system and investigate the weak scaling parallel performance.

## The benchmark system

The benchmark system we will use to investigate the weak scaling is a box of water. This is a simple system than can be created for different system sizes.

The initial box size is 3nm x 3nm x 3nm and contains 884 water molecules. We have provided input files for this system ``water_x1.tpr`` and multiple larger systems with 2, 4, 8, ... up to 8096 times the volume (``water_x2.tpr``, ``water_x4.tpr``, ``water_x8.tpr``, ... , ``water_8096.tpr``).

```{figure} ./images/gmx_water.png
---
class: with-border
---
Water boxes. The x1 box contains 884 water molecules, the x16 box contains ~14000.
```


The water model used is TIP3P, this has three sites per molecule, corresponding to the two hydrogen and one oxygen molecule in H20. There are many other water models that can be used depending on the type of simulation, for our purposes of benchmarking TIP3P is sufficient. For more information see:
[https://en.wikipedia.org/wiki/Water_model](https://en.wikipedia.org/wiki/Water_model)

## Running the benchmark

The benchmarks are designed such that ``water_x1.tpr`` is a suitable size for running on 1 core. (GROMACS works best with ~1000 atoms per CPU)

An example script to run the benchmark on {{ machine_name }} is shown below.

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise_gmx/gmx_slurm_part2.md\n```'.replace("REPLACE",machine_name) }}

Once again the important number is the ns/day figure.


## Things to investigate
- You should run the benchmarks systems where the number of cores used scales with the system size. I.e run ``water_x1.tpr`` with 1 core, ``water_x2.tpr`` with 2 cores, ``water_128.tpr`` with 128 cores etc. This investigates the weak scaling

- Calculate the weak efficiency. This is given by:

$$
E(N) = \frac{\text{Performance}(N \text{ cores}, \text{ System size } N)}{\text{Performance}(1 \text{ core}, \text{ system size 1})}
$$



- Plot the results and look at the difference between the intra-node and inter-node scaling.

We have plotted our results for version 2021.3 of GROMACS on ARCHER2 below:

```{figure} ./images/gmx_weak_scaling.svg
---
class: with-border
---
Weak scaling efficiency results for the benchmark system on ARCHER2. 
```