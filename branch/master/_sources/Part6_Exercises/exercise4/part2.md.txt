## Traffic model simulation: Threaded computation

Having run this example in serial we can now explore running the simulation in parallel in order to test how to speed up getting the average verlocity of cars changes with traffic density.
### The source code

In this exercise we will be using the traffic simulaion program. The source code is available in  Git repository EPCC-Exercises we have already downloaded.

We will now be looking at the multi threaded version located in the ``C-OMP`` folder. The uses openmp to parallelise the execution of the model.

### Compiling the source code

We will compile the openmp version of the source code using a Makefile.

Move into the ``C-OMP`` directory and list the contents.

>```
>    cd C-OMP
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

### Running: Threaded code

We can run the threaded program directly on the login nodes,

>```
>    ./traffic 0.52
>```

Again the argument is setting the target traffic density of cars for the model.

By default the number of threads used is 256. This can be changed by setting,

```
export OMP_NUM_THREADS=8
```
for example this sets the number of threads to 8.


Running this quickly on the login node leads to 

Output:
```
Length of road is 32000000
Number of iterations is 100
Target density of cars is 0.520000
Running on 8 thread(s)
Initialising road ...
...done
Actual density of cars is 0.519982

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

Time taken was  2.446868 seconds
Update rate was 1307.794488 MCOPs
```

### Running: Batch system

Now we have tested the simulation on the login node we now neeed to set this up to run on the compute nodes.

```{note}
In general jobs should not be run on the login nodes. It is however reasonable to run small and short duration tests to validate your software before submitting to the compute noe queues.
```

Example substitution:

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise4/ex4_omp.md\n```'.replace("REPLACE",machine_name) }}

### Conclusion

Executing this for different values of the traffic density can now be acheived much more quickly as each simulation is fast than the serial case and we can submit jobs to multiple nodes simultaneously allowing us to run in parallel reducing our time to science. 

Compare the time taken to perform calculations with the same traffic densities using the serial code and compare the time to solution. Plotting this data can be used to calculate the speed up of the threaded code over the serial version. 

The simulation bares out a plot of what we expected that as te traffic density increases the speed of the traffic drops.


