```
#!/bin/bash

# Slurm job options (name, compute nodes, job time)
#SBATCH --job-name=Hello-MPI
#SBATCH --time=00:20:00
#SBATCH --exclusive
#SBATCH --nodes=4
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=1

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
NODES=$SLURM_JOB_NUM_NODES
CORES=$((NODES*288))
THREADS=$OMP_NUM_THREADS

export OMP_PLACES=cores

# Launch the parallel job
#   1 MPI process per node
#   srun picks up the distribution from the sbatch options
srun --hint=nomultithread --distribution=block:block ./hello-MPI your-name > MPI-${NODES}nodes-${CORES}cores-${THREADS}threads.${SLURM_JOBID}.out
```