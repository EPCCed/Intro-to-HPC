
```{code-block} bash
---
caption: archer2.slurm
---
#!/bin/bash

#SBATCH --job-name=gmx_bench
#SBATCH --nodes=1
#SBATCH --tasks-per-node=128
#SBATCH --cpus-per-task=1
#SBATCH --time=00:10:00

# Replace [budget code] below with your project code (e.g. t01)
#SBATCH --account=z19
#SBATCH --partition=standard
#SBATCH --qos=standard

# Setup the environment
module load gromacs

export OMP_NUM_THREADS=1 
srun --distribution=block:block --hint=nomultithread gmx_mpi mdrun -s bench_465kHBS.tpr -v
```
