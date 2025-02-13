# Part 2: Compiling and running Hello world!


This example is meant to get you used to the command line envrionment of a high performance computer and submitting jobs to the batch system, while learning about the hardware of a HPC system.

In the following we are going to look at variety of hello world programs and look at the two most common types of parallelism in the HPC world. 

One that takes advantage of shared memory and distributed memory parts of a HPC system.


## Serial

The serial example runs a job on a node with a single process. This is the same as if you ran on your local machine.

For those who are interested the code we are executing is,

```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <limits.h>

int main(int argc, char* argv[])
{

    char hostname[HOST_NAME_MAX];
    char username[LOGIN_NAME_MAX];
    gethostname(hostname, HOST_NAME_MAX);
    getlogin_r(username, LOGIN_NAME_MAX);

    printf("Hello World!\n");
    printf("Hello %s, this is %s.\n", username, hostname);

}

```
this is very simple code but it will report where it is running from.

To run this example use the following batch script for {{ machine_name }}:

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


This will just say hello from a single node on {{ machine_name }} and report the node's name.


---

## Threading

This threaded example runs on as many threads on a node as you allow it to.


The code is a little more complex than the last example in order to run multiple copies of the responce from each thread.

```

#include <omp.h>

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <limits.h>

int main(int argc, char* argv[])
{

  char hostname[HOST_NAME_MAX];
  char username[LOGIN_NAME_MAX];
  gethostname(hostname, HOST_NAME_MAX);
  getlogin_r(username, LOGIN_NAME_MAX);

  printf("Hello World!\n");

  #pragma omp parallel
  {
    printf("Hello %s, this is node %s responding from thread %d\n", username, hostname,
           omp_get_thread_num());
  }

}


```

In order to run this on a hpc node we can use the following script,


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


Here we continue to run on a single process, threads take advantage of the shared memory aspect of a HPC system and cannot communicated between destinct nodes but can work as a team on a single node to improve performance. 

If you run this with multiple processes then it will still work but without MPI communucation these processes will be entirly independent and can not communicate information.

---

## MPI

MPI is a message passing interface, this allow for messages to be sent by multiple instances on the program running on different nodes each being controlled by a seperate instance of the operating system.

In an MPI program when start the program we tell it how mant copies of the code is running and 


This MPI example each process says hello in the programs and states which node it is running on and which .

```

some code (this code need tidying)

```

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
