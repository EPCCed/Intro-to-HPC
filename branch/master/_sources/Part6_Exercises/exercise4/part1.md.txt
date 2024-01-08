## Traffic model simulation: Serial Execution

As just discussed the aim of this simulation is to understand how traffic density has an effect on average traffic speed.


### Downloading the source code

In this exercise we will be using the traffic simulaion program. The source code is available in the same Github repository as the other examples.

To download the code you will need to clone the repository. To do this execute the following command

>```
>    git clone https://github.com/EPCCed/EPCC-Exercises
>```

The output will look similar to this
```
    Cloning into 'EPCC-Exercises'...
    remote: Enumerating objects: 21, done.
    remote: Counting objects: 100% (21/21), done.
    remote: Compressing objects: 100% (15/15), done.
    remote: Total 21 (delta 7), reused 16 (delta 5), pack-reused 0
    Receiving objects: 100% (21/21), 314.18 KiB | 2.51 MiB/s, done.
    Resolving deltas: 100% (7/7), done.

```

You will now have a folder called ``traffic``. Change directory into it and list the contents

>```
>    cd EPCC-Exercises/traffic/C
>    ls
>```

Output
```
  C-MPI  C-OMP  C-SER
```

There are several version of the code, a serial version and a number of parallel versions. Initially we will be looking at the serial version located in the ``C-SER`` folder.

### Setting the Environment

Before compiling the source code for the example make sure that the relivant environment scripts has been sourced to set the correct environment parameters for compiling on this machine.

For example of Machine {{ machine_name }},

```
    source EPCC-Exercises/Env/env- {{ machine_name }} .sh
```

This makes sure that each example compiles for the system you are using to run the examples.

### Compiling the source code

We will compile the serial version of the source code using a Makefile.

Move into the ``C-SER`` directory and list the contents.

>```
>    cd C-SER
>    ls
>```

Output:
```
    traffic.c  traffic.h  trafficlib.c  uni.c  uni.h  Makefile
```

You will see that there are various code files. The Makefile contains the commands to compile them together to produce the executable program. To use the Makefile type ``make`` command.

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

### Running the serial code

We can run the serial program directly on the login nodes

>```
>    ./traffic 0.52
>```

The argument is setting the target traffic density of cars for the model.

Output:
```
Length of road is 32000000
Number of iterations is 100
Target density of cars is 0.520000
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

Time taken was  11.418155 seconds
Update rate was 280.255431 MCOPs
```

The result we are interested in this the final avarge verlocity that is reported at iteration 100. 

Run this example with multiple different values of the Target density and see how your results compare to the plot relating traffic density to average verlocity from the section on the traffic model.

