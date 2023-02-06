```

#!/bin/bash

# Slurm job options (name, compute nodes, job time)
#SBATCH --job-name=Hello-HYB
#SBATCH --time=00:20:00
#SBATCH --nodes=4
#SBATCH --tasks-per-node=2
#SBATCH --cpus-per-task=2

# Replace [budget code] below with your budget code (e.g. t01)
#SBATCH --account=[budget code]
#SBATCH --qos=standard
#SBATCH --partition=standard

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
NODES=$SLURM_JOB_NUM_NODES
CORES=$((NODES*128))
THREADS=$OMP_NUM_THREADS

export OMP_PLACES=cores

srun --hint=nomultithread --distribution=block:block ./hello-HYB your-name > HYBRID-${NODES}nodes-${CORES}cores-${THREADS}threads-run.${SLURM_JOBID}.out

```