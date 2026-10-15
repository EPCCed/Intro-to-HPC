
```{code-block} bash
---
caption: cirrus.slurm
---
#!/bin/bash

#SBATCH --job-name=gmx_bench
#SBATCH --nodes=1
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=1
#SBATCH --time=00:10:00

# Replace [budget code] below with your project code (e.g. t01)
#SBATCH --account=[budget code]
#SBATCH --partition=standard
#SBATCH --qos=standard
# Change to the submission directory
cd $SLURM_SUBMIT_DIR

# Setup the environment
module load gromacs

export OMP_NUM_THREADS=1 
srun --hint=nomultithread --distribution=block:block gmx_mpi mdrun -s water_x1.tpr -v
```

Make sure that ``nodes`` x ``tasks-per-node`` is close to the system size.
i.e for ``water_x256.tpr`` use ``--nodes=1`` and ``--tasks-per-node=256`` 