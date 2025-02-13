#### archer2.slurm:

To run the simulation using the compule nodes you need to a job script,

``` slurm
#!/bin/bash

#SBATCH --job-name=sharpen
#SBATCH --nodes=1
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=4
#SBATCH --time=00:01:00

# Replace [budget code] below with your project code (e.g. t01)
#SBATCH --account=[budget code]
#SBATCH --partition=standard
#SBATCH --qos=standard

# Setup the batch environment
module load epcc-job-env

# Set the number of threads to the CPUs per task
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

# Launch the parallel job
srun --hint=nomultithread --distribution=block:block ./traffic 0.52
```

This is an OpenMP program so we control the number of parallel threads used with the ``--cpus-per-task`` variable.

To submit the job to run on the compute nodes we use the ``sbatch`` command

```
sbatch archer2.slurm
```

Output:
```
Submitted batch job 1793266
```
Where the number is the unique job ID.

```{note}
    On ARCHER2 you must submit jobs from the ``/work`` filesystem.
```


#### Monitoring the batch job
The slurm command ``squeue`` can be used to show the status of the jobs. Without any options or arguments it lists all jobs known by the scheduler.
```
squeue
```

To show just your jobs add  the ``-u $USER`` option
```
squeue -u $USER
```
Note that for this example it runs very quickly so you may not see it in the queue before it finishes running.

#### Finding the output
The Slurm system places the output from your job in a file called ``slurm-<jobID>.out``. You can view it using the ``cat`` command

```
cat slurm-1793266.out

Length of road is 32000000
Number of iterations is 100
Target density of cars is 0.520000
Running on 4 thread(s)
Initialising road ...
...done
Actual density of cars is 0.519982

At iteration 10 average velocity is 0.789461
At iteration 20 average velocity is 0.837157
At iteration 30 average velocity is 0.858209
At iteration 40 average velocity is 0.870573
At iteration 50 average velocity is 0.878940
At iteration 60 average velocity is 0.885087
At iteration 70 average velocity is 0.889761
At iteration 80 average velocity is 0.893441
At iteration 90 average velocity is 0.896447
At iteration 100 average velocity is 0.898955

Finished

Time taken was  2.779974 seconds
Update rate was 1151.089820 MCOPs

```