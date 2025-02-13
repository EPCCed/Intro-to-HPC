```
#!/bin/bash

# Slurm job options (name, compute nodes, job time)
#SBATCH --job-name=Hello-SER
#SBATCH --time=00:20:00
#SBATCH --exclusive
#SBATCH --nodes=1
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=1

# Replace [budget code] below with your budget code (e.g. t01)
#SBATCH --account=[budget code]
# We use the "standard" partition as we are running on CPU nodes
#SBATCH --partition=standard
# We use the "standard" QoS as our runtime is less than 4 days
#SBATCH --qos=standard

# Load the default HPE MPI environment
module load mpt
module load intel-20.4/compilers

# Change to the submission directory
cd $SLURM_SUBMIT_DIR

# Set the number of threads to 1
#   This prevents any threaded system libraries from automatically
#   using threading.
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
NODES=$SLURM_JOB_NUM_NODES
CORES=$((NODES*128))
THREADS=$OMP_NUM_THREADS

export OMP_PLACES=cores

# Launch the parallel job
#   Using 144 MPI processes and 36 MPI processes per node
#   srun picks up the distribution from the sbatch options
srun ./hello-SER your-name  > SERIAL-${NODES}nodes-${CORES}cores-${THREADS}threads.${SLURM_JOBID}.out
```