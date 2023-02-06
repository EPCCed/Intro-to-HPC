```

#!/bin/bash

#SBATCH --job-name=Hello-SER
#SBATCH --nodes=1
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=1
#SBATCH --time=00:01:00

# Replace [budget code] below with your project code (e.g. t01)
#SBATCH --account=[budget code]
#SBATCH --partition=standard
#SBATCH --qos=standard

# Set the number of threads to the CPUs per task
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
NODES=$SLURM_JOB_NUM_NODES
CORES=$((NODES*128))
THREADS=$OMP_NUM_THREADS

export OMP_PLACES=cores

# Launch the parallel job
srun --hint=nomultithread --distribution=block:block ./hello-SER your-name > SERIAL-${NODES}nodes-${CORES}cores-${THREADS}threads.${SLURM_JOBID}.out

```