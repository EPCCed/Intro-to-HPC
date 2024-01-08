## Traffic model simulation: Message passing computation

Having run this example in serial and multiple threads we can now explore running the simulation using a message passing modle to determine verlocity of cars changes with traffic density.



### The source code

In this exercise we will be using the traffic simulaion program. The source code is available in  Git repository EPCC-Exercises we have already downloaded.

We will now be looking at the message passing version located in the ``C-MPI`` folder. This version uses MPI to parallelise the execution of the model.

### Compiling the source code

We will compile the MPI version of the source code using a Makefile.

Move into the ``C-MPI`` directory and list the contents.

>```
>    cd C-MPI
>    ls
>```

Output:
```
    traffic.c  traffic.h  trafficlib.c  uni.c  uni.h  Makefile
```

You will see that there are various code files. The Makefile contains the commands to compile them together to produce the executable program. To use the Makefile type ``make`` command. 

```{note}

We don't need to set a new environment file as the 'EPCC-Exercises/Env/env-{ machine_name }.sh' we sourced earlier also set the correct environmnet parmeters to correctly build the parallel examples of the code on { machine_name }. 

```

>```bash
>    make
>```

Output:
```
cc -g -O3 -c traffic.c
cc -g -O3 -c trafficlib.c
cc -g -O3 -c uni.c
cc -g -O3 -o traffic traffic.o trafficlib.o uni.o -lm
```

This should produce an executable file called ``traffic``.  

### Running: Message passing code

While a we can have multiple processes per node and run on a single node one of the main aims of message passing codes is to spread the calculation over multple nodes. This allows large simulations to be performed as more memory and processors are avalible for a given job.

Her we just look at an example submitted to the bastch system.

Example substitution:

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise4/ex4_mpi.md\n```'.replace("REPLACE",machine_name) }}

#### archer2.slurm:

To run the simulation using the compule nodes you need to a job script,

``` slurm
#!/bin/bash

#SBATCH --job-name=sharpen
#SBATCH --nodes=2
#SBATCH --tasks-per-node=2
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
srun --hint=nomultithread --distribution=block:block ./traffic 0.52
```

This is an MPI program so we control the number of nodes used with the ``--nodes`` variable. We control the number of processes per node with ``--tasks-per-node``.

To submit the job to run on the compute nodes we use the ``sbatch`` command

```
sbatch archer2.slurm
```

Output:
```
Submitted batch job 1793266
```
Where the number is the unique job ID.

```{note}
    On ARCHER2 you must submit jobs from the ``/work`` filesystem.
```


#### Monitoring the batch job
The slurm command ``squeue`` can be used to show the status of the jobs. Without any options or arguments it lists all jobs known by the scheduler.
```
squeue
```

To show just your jobs add  the ``-u $USER`` option
```
squeue -u $USER
```
Note that for this example it runs very quickly so you may not see it in the queue before it finishes running.

#### Finding the output
The Slurm system places the output from your job in a file called ``slurm-<jobID>.out``. You can view it using the ``cat`` command

```
cat slurm-1793266.out

Running message-passing traffic model
Running message-passing traffic model
Running message-passing traffic model

Length of road is 32000000
Number of iterations is 100
Target density of cars is 0.520000
Running on 4 processes
Initialising road ...
...done
Actual density is 0.519982
Scattering data ...
... done

At iteration 10 average velocity is 0.789461
At iteration 20 average velocity is 0.837157
At iteration 30 average velocity is 0.858209
At iteration 40 average velocity is 0.870573
At iteration 50 average velocity is 0.878940
At iteration 60 average velocity is 0.885087
At iteration 70 average velocity is 0.889761
At iteration 80 average velocity is 0.893441
At iteration 90 average velocity is 0.896447
At iteration 100 average velocity is 0.898955

Finished

Time taken was  2.183829 seconds
Update rate was 1465.316302 MCOPs

```

### Conclusion

This simulation should also produce the same plot of average verlocity verses traffic density as the other two examples. The version of the code allows for multiple nodes to be used in a simulation. This gives us the ability to spread a calculation over more nodes to gain a speed up and simulate larger systems. This is where high performance computing power comes from the ability to coordiante multiple sperate nodes to perform a single simulation allows for more complex systems to be studies over and above what a single server can handle.