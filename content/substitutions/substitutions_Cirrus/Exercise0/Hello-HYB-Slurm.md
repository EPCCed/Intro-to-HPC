```
#!/bin/bash

# Slurm job options (name, compute nodes, job time)
#SBATCH --job-name=Hello-HYB
#SBATCH --time=00:20:00
#SBATCH --exclusive
#SBATCH --nodes=4
#SBATCH --tasks-per-node=2
#SBATCH --cpus-per-task=2

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
#   2 MPI processes per node with 2 OpenMP threads each
#   srun picks up the distribution from the sbatch options
srun --hint=nomultithread --distribution=block:block ./hello-HYB your-name > HYBRID-${NODES}nodes-${CORES}cores-${THREADS}threads.${SLURM_JOBID}.out

```