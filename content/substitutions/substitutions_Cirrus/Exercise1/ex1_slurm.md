```
#!/bin/bash

# Slurm job options (name, compute nodes, job time)
#SBATCH --job-name=sharpen
#SBATCH --time=00:20:00
#SBATCH --nodes=1
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=4

# Replace [budget code] below with your budget code (e.g. t01)
#SBATCH --account=[budget code]
# We use the "standard" partition as we are running on CPU nodes
#SBATCH --partition=standard
# We use the "standard" QoS as our runtime is less than 4 days
#SBATCH --qos=standard

# PrgEnv-cray loaded by default (cray-mpich + CCE + cray-libsci)
# No module commands needed in scripts

# Change to the submission directory
cd $SLURM_SUBMIT_DIR

# Set the number of threads to the CPUs per task
#   This prevents any threaded system libraries from automatically
#   using threading.
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export SRUN_CPUS_PER_TASK=$SLURM_CPUS_PER_TASK

export OMP_PLACES=cores

# Launch the parallel job
#   Using 4 OpenMP threads
#   srun picks up the distribution from the sbatch options
srun --hint=nomultithread --distribution=block:block ./sharpen
```

This is an OpenMP program so we control the number of parallel threads used with the ``--cpus-per-task`` variable.

To submit the job to run on the compute nodes we use the ``sbatch`` command

```
sbatch ex1_slurm.md
```

Output:
```
Submitted batch job <jobid>
```
Where the number is the unique job ID.

```{note}
    On Cirrus you must submit jobs from the ``/work`` filesystem.
```


### Monitoring the batch job
The slurm command ``squeue`` can be used to show the status of the jobs. Without any options or arguments it lists all jobs known by the scheduler.
```
squeue
```

To show just your jobs add  the ``-u $USER`` option
```
squeue -u $USER
```
Note that for this example it runs very quickly so you may not see it in the queue before it finishes running.

### Finding the output
The Slurm system places the output from your job in a file called ``slurm-<jobID>.out``. You can view it using the ``cat`` command

```
cat slurm-<jobid>.out
```


Output:
```
Image sharpening code running on 4 thread(s)

Input file is: fuzzy.pgm
Image size is 564 x 770

Using a filter of size 17 x 17

Reading image file: fuzzy.pgm
... done

Starting calculation ...
Thread 0 on core 0
Thread 1 on core 1
Thread 2 on core 2
Thread 3 on core 3
... finished

Writing output file: sharpened.pgm

... done

Calculation time was <calc_time> seconds
Overall run time was <total_time> seconds
```

To control the number of threads you can edit the ``#SBATCH --cpus-per-task=4`` variable to a different number and resubmit the job.

Because this is an OpenMP program it will not scale beyond one node which has 288 cores on Cirrus EX4000.
