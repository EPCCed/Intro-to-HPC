# Part 1: Strong Scaling 

In this section you will run a benchmark simulation and investigate the strong scaling parallel performance.


## The benchmark system

The system we will used from the HECBIOSIM benchmark suite:
[https://www.hecbiosim.ac.uk/access-hpc/benchmarks](https://www.hecbiosim.ac.uk/access-hpc/benchmarks)

We will focus on the 465K atom system:
  - hEGFR Dimer of 1IVO and 1NQL. 
  - Total number of atoms = 465,399. 
  - Protein atoms = 21,749  Lipid atoms = 134,268  Water atoms = 309,087  Ions = 295. 


```{figure} ./images/465K-H1-H1.png
---
class: with-border
---
465K atom system from HECBIOSIM benchmark suite. The orange molecule is the protein. The purple and gray molecules are the lipid-membrane. The box is filled with water but this is not shown for clarity.
```
The input file can be obtained from [https://www.hecbiosim.ac.uk/access-hpc/benchmarks](https://www.hecbiosim.ac.uk/access-hpc/benchmarks), or from our git repo: #TODO


## Running the benchmark

An example script to run the benchmark on {{ machine_name }} is shown below.

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise_gmx/gmx_slurm_part1.md\n```'.replace("REPLACE",machine_name) }}

The bottom of the ``md.log`` file will contain the performance timings, e.g:

```bash
               Core t (s)   Wall t (s)        (%)
       Time:    12283.520       95.966    12799.9
                 (ns/day)    (hour/ns)
Performance:       18.008        1.333
```

The most useful numbers to us are the wall-time ``95.966s`` , and the performance: ``18.008 ns/day`` which tells us how much simulation time (in nano-seconds) can be simulated for 1 day of wall-clock time.

## Things to investigate
- You should vary the number of nodes/CPUs and plot the performance. This investigates the strong scaling of the program.
- What node count would you use for a long production simulation?
- Previous benchmarks for this system can be found here:
[https://www.hecbiosim.ac.uk/access-hpc/our-benchmark-results/archer2-benchmarks](https://www.hecbiosim.ac.uk/access-hpc/our-benchmark-results/archer2-benchmarks)

We have plotted our results for version 2021.3 of gromacs on ARCHER2 here:


```{figure} ./images/gmx_strong_scaling.svg
---
class: with-border
---
Performance results for the benchmark system on ARCHER2. The performance is measured in nanoseconds per day.
```

## Comparing results with Amdahl's law

Amdahl's law characterizes the speedup of a parallel program. It states that the speedup for $N$ processors $S(N)$ is dependent on the serial $s$ and parallel $p$ portions of the code.

$$
S(N) = \frac{T(1)}{T(N)} = \frac{1}{s+\frac{p}{N}}
$$

We have plotted Amdahl's law for $p=$ 99%, 99.9%, and 99.95% to compare against the measured results.



```{figure} ./images/gmx_amdahl.svg
---
class: with-border
---
Performance results for the benchmark system on ARCHER2. The performance is measured in nanoseconds per day.
```

We can see that these results are similar to the p=99.9% curve. Suggesting that 99.9% of the code is parallel. However, it in reality it is not this simple. Amdahl's law assumes perfect load balance, this is not usually the case. The ``md.log`` file reports the load balance for this benchmark. E.g. for our run using 4 nodes:

```
Dynamic load balancing report:
 DLB was permanently on during the run per user request.
 Average load imbalance: 16.0%.
 The balanceable part of the MD step is 78%, load imbalance is computed from this.
 Part of the total run time spent waiting due to load imbalance: 12.5%.
 Steps where the load balancing was limited by -rdd, -rcon and/or -dds: X 0 % Y 0 % Z 0 %
 Average PME mesh/force load: 0.723
 Part of the total run time spent waiting due to PP/PME imbalance: 6.2 %

NOTE: 12.5 % of the available CPU time was lost due to load imbalance
      in the domain decomposition.
      You can consider manually changing the decomposition (option -dd);
      e.g. by using fewer domains along the box dimension in which there is
      considerable inhomogeneity in the simulated system.
NOTE: 6.2 % performance was lost because the PME ranks
      had less work to do than the PP ranks.
      You might want to decrease the number of PME ranks
      or decrease the cut-off and the grid spacing.
```

We can see that significant load imbalance is reported.