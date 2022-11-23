# Part 2: Download,Compile, Run

This page covers how to download, compile, and run code.


## Downloading the source code

In this exercise we will be using a image sharpening program. The source code is available in a Github repository.


To download the code you will need to clone the repository. To do this execute the following command,

>```
>    git clone https://github.com/EPCCed/EPCC-Exercises
>```

Alternatively you might have already cloned the repository as part of another exercise in which case skip this step.

If you clone the repository the output will look similar to this
```
    Cloning into 'EPCC-Exercises'...
    remote: Enumerating objects: 21, done.
    remote: Counting objects: 100% (21/21), done.
    remote: Compressing objects: 100% (15/15), done.
    remote: Total 21 (delta 7), reused 16 (delta 5), pack-reused 0
    Receiving objects: 100% (21/21), 314.18 KiB | 2.51 MiB/s, done.
    Resolving deltas: 100% (7/7), done.

```

You will now have a folder called ``sharpen``. Change directory into it and list the contents

```
    cd EPCC-Exercises/Sharpen/C
    ls
```

Output:
```
  C-Common  C-HYB  C-MPI  C-OMP  C-SER  C-SHM  README.md
```

There are several version of the code, a serial version and a number of parallel versions. Initially we will be looking at the serial version located in the ``C-SER`` folder.

## Compiling the source code

We will compile the serial version of the source code using a Makefile.

Move into the ``C-SER`` directory and list the contents.

>```
>    cd C-SER
>    ls
>```

Output:
```
    Makefile  cio.c  dosharpen.c  filter.c  fuzzy.pgm  sharpen.c  sharpen.h  utilities.c  utilities.h
```

You will see that there are various code files. The Makefile contains the commands to compile them together to produce the executable program. To use the Makefile type ``make`` command.

>```bash
>    make
>```

Output:
```
cc -g -c sharpen.c
cc -g -c dosharpen.c
cc -g -c filter.c
cc -g -c cio.c
cc -g -c utilities.c
cc -g -o sharpen sharpen.o dosharpen.o filter.o cio.o utilities.o -lm
```

This should produce an executable file called ``sharpen``.  

## Running the serial program

We can run the serial program directly on the login nodes

>```
>    ./sharpen
>```

Output:
```
    Image sharpening code running in serial

    Input file is: fuzzy.pgm
    Image size is 564 x 770

    Using a filter of size 17 x 17

    Reading image file: fuzzy.pgm
    ... done

    Starting calculation ...
    Program on core 0-255
    ... finished

    Writing output file: sharpened.pgm

    ... done

    Calculation time was 3.697039 seconds
    Overall run time was 3.923524 seconds
```


## Viewing the images

To view the images on the remote machine you will need to make sure you an X window client installed on your local machine and you have logged into the remote machine with X forwarding enabled.

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise1/view_pgm.md\n```'.replace("REPLACE",machine_name) }}

Alternatively you can download the files to your local machine via SSH (``scp``, ``rysnc``, or ``stfp``) and open them with an image viewing program e.g. preview on MacOS.

The images should look like this:

```{figure} ./images/both_images.png
---
class: with-border
alt: Fuzzy and sharpened image
---

Fuzzy and sharpened image.
```


In the next step we will run the parallel version of the code on the compute nodes using a batch submission system.