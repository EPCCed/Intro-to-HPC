## Traffic model simulation: Message passing computation

Having run this example in serial and multiple threads we can now explore running the simulation using a message passing model to determine velocity of cars changes with traffic density.



### The source code

In this exercise we will be using the traffic simulation program. The source code is available in  Git repository EPCC-Exercises we have already downloaded.

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

We don't need to set a new environment file as the 'EPCC-Exercises/Env/env-{ machine_name }.sh' we sourced earlier also set the correct environment parameters to correctly build the parallel examples of the code on { machine_name }. 

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

While a we can have multiple processes per node and run on a single node one of the main aims of message passing codes is to spread the calculation over multiple nodes. This allows large simulations to be performed as more memory and processors are available for a given job.

Her we just look at an example submitted to the batch system.

Example substitution:

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise4/ex4_mpi.md\n```'.replace("REPLACE",machine_name) }}


### Conclusion

This simulation should also produce the same plot of average velocity verses traffic density as the other two examples. The version of the code allows for multiple nodes to be used in a simulation. This gives us the ability to spread a calculation over more nodes to gain a speed up and simulate larger systems. This is where high performance computing power comes from the ability to coordinate multiple separate nodes to perform a single simulation allows for more complex systems to be studies over and above what a single server can handle.