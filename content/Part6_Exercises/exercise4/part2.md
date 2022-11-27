## Traffic model simulation: shared memory computation

Having run this example in serial we can now explore running the simulation in parallel in order to test how to efficently get the average verlocity of cars changes with traffic density the fastest.
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






## Conclusion

- The simulation bares out a plot of what we expected that as te traffic density increases the speed of the traffic drops.