# Part 2: Compiling and running Hello world!


This example is both meant to get you used to the command line envrionment of a high performance computer aswell as submitting jobs to the batch system, while learning about the hardware of a HPC system.

In the following programs we are going to look at a hello world program and look at two types of parallelism both on  both shared memory and distributed memory systems.


## Serial

The serial example runs a job on a node with a single process. This is the same as if you ran on your local machine.

- code 

- script example

```

#!/bin/bash

#SBATCH --job-name=sharpen
#SBATCH --nodes=1
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=1
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
srun --hint=nomultithread --distribution=block:block ./hello-SER

```

- expected result

Focus on how the output is presented and how might this be controled.

## MPI

This MPI example each process says hello in the programs and states where it is from.

- code 

- script example 1

```

#!/bin/bash

#SBATCH --job-name=sharpen
#SBATCH --nodes=1
#SBATCH --tasks-per-node=1
#SBATCH --cpus-per-task=1
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
srun --hint=nomultithread --distribution=block:block ./hello-MPI

```


In the above script we assume one process per node this means we see one process per node responding to our welcome.


- script example 2

```

#!/bin/bash

#SBATCH --job-name=sharpen
#SBATCH --nodes=1
#SBATCH --tasks-per-node=4
#SBATCH --cpus-per-task=1
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
srun --hint=nomultithread --distribution=block:block ./hello-MPI

```


In this updated script we have increased the number of processes per node. this means we see multiple processes respond per node.


- expected result

## Threading

This threaded example has each thread in a process say hello.


- code 

- script example

```

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
srun --hint=nomultithread --distribution=block:block ./hello-THRD

```

- expected result

If you run this with multiple processes then it will still work but without MPI communucation these processes will be entirly independent and can not communicate information.

## Hybrid

This hybid code has each thread in each process say hello.

- code 

- script example


In this exampe script focus on the multiple threads per process.


- expected result

## Conclusion

The point of this exercise is to show that there are different ways to orginise a calculation and the location of operation changes depending on how we spread out the calcualtion over the nodes.

A few other tests you can try:

- Run the serial code with more than one process what do you see?
- Running the threaded code with more than one process what do you see?
- running the MPI code on a single node with multiple processes what you you see?

Remember that message passing codes cost an overhead to send messages and threaded codes cant scale beyond the size of a single node.
