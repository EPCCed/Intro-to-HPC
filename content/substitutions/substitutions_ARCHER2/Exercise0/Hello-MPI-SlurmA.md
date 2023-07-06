```

#!/bin/bash

#SBATCH --job-name=Hello-MPI
#SBATCH --nodes=4
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=1
#SBATCH --time=00:20:00

# Replace [budget code] below with your project code (e.g. t01)
#SBATCH --account=[budget code]
#SBATCH --partition=standard
#SBATCH --qos=standard

# Set the number of threads to the CPUs per task
export SRUN_CPUS_PER_TASK=$SLURM_CPUS_PER_TASK
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

NODES=$SLURM_JOB_NUM_NODES
CORES=$((NODES*128))
THREADS=$OMP_NUM_THREADS

export OMP_PLACES=cores

echo "job start"

# Launch the parallel job
srun --hint=nomultithread --distribution=block:block ./hello-MPI YOUR-NAME-HERE > MPI-${NODES}nodes-${CORES}cores-${THREADS}threads.${SLURM_JOBID}.out

echo "job complete"

```

Place this bash code into a a file called `Hello_MPI_Slurm.sh` in the same directory as the previous code and replace `YOUR_NAME_HERE` with your own input.

To submit this job run,

```
sbatch Hello_MPI_Slurm.sh
```

This should return two files as output,

- The first file name begins with `MPI-...` is the log file from the job and contains a message produced by the code at run time.
- The second file name begins with `slurm` is the output from the script used to submit the job.
