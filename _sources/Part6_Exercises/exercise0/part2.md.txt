# Part 2: Compiling and running Hello world!


This example is meant to get you used to the command line environment of a high performance computer and submitting jobs to the batch system, while learning about the hardware of a HPC system.

In the following we are going to look at variety of hello world programs and look at the two most common types of parallelism in the HPC world. 

One that takes advantage of shared memory and one that uses distributed memory in a HPC system. We will then look at how they can be combined but first we start with a simple serial code.

## Serial

The serial example runs a job on a node with a single process. This is the same as if you ran on your local machine.

For those who are interested the code we are executing is,

```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <limits.h>
#include <string.h>

int main(int argc, char* argv[])
{

    // Check input argument

    if(argc != 2)
    {
        printf("Required one argument `name`.\n");
        return 1;
    }

    // Receive argument

    char* iname = (char *)malloc(strlen(argv[1]));

    strcpy(iname, argv[1]);

    // Get the name of the node we are running on

    char hostname[HOST_NAME_MAX];
    gethostname(hostname, HOST_NAME_MAX);

    // Hello World message

    printf("Hello World!\n");

    // Message from the node to the user

    printf("Hello %s, this is %s.\n", iname, hostname);

}

```
This is a simple C code but it will say hello to you and report which node it is running from.

To try this example yourself you will first need to compile the example code.

If the file that contains the above code is called `helloWorldSerial.c` then to compile on {{ machine_name }} use command,

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise0/Hello-SER-Compile.md\n```'.replace("REPLACE",machine_name) }}

To run this example using the compute nodes via the job queue, use the following bash script written for {{ machine_name }},

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise0/Hello-SER-Slurm.md\n```'.replace("REPLACE",machine_name) }}

This example is small enough that it can be run on the login nodes of {{ machine_name }} by running,

 ```
 ./hello-SER
 ```

 How does this differ from when you run using the batch script?


---

## Threading

This threaded example runs on as many threads on a node as you allow it to.


The code is a little more complex than the last example in order to run multiple copies of the response from a number of threads on the node we use OpenMP to parallelise the code.

```

#include <omp.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <limits.h>
#include <string.h>


int main(int argc, char* argv[])
{

  // Check input argument

  if(argc != 2)
  {
      printf("Required one argumnet `name`.\n");
      return 1;
  }

  // Receive argument

  char* iname = (char *)malloc(strlen(argv[1]));

  strcpy(iname,argv[1]);

  // Get the name of the node we are running on

  char hostname[HOST_NAME_MAX];
  gethostname(hostname, HOST_NAME_MAX);

  // Hello World message

  printf("Hello World!\n");

  // Message from each thread on the node to the user

  #pragma omp parallel
  {
    printf("Hello %s, this is node %s responding from thread %d\n", iname, hostname,
           omp_get_thread_num());
  }

}

```

To try this example yourself you will first need to compile the example code.

If the file that contains the above code is called `helloWorldThreaded.c` then to compile on {{ machine_name }} use command,

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise0/Hello-THRD-Compile.md\n```'.replace("REPLACE",machine_name) }}

In order to run this on a {{ machine_name }} node we can use the following script,

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise0/Hello-THRD-Slurm.md\n```'.replace("REPLACE",machine_name) }}

As in the serial case we have run a single process but now the process runs a number of threads. Threaded codes can take advantage of the shared memory aspect of a HPC systems to pass data between each thread but cannot communicated between distinct nodes. 

If you run this code on multiple processes then it will still work but without MPI communication these processes will be entirely independent and are not able to communicate information.

---

## MPI

MPI is a message passing interface, this allow for messages to be sent by multiple instances of the program running on different nodes to each other. Each instance of the program is controlled by a separate instance of the operating system.

This MPI example each process says hello in the programs and states which node it is running on and which process in the group it is.

```

#include <stdio.h>
#include <mpi.h>
#include <iostream>
#include <string.h>

int main(int argc, char *argv[])
{
    // Check input argument

    if(argc != 2)
    {
        printf("Required one argument `name`.\n");
        return 1;
    }

    // Receive arguments

    char* iname = (char *)malloc(strlen(argv[1])+1);
    char* iname2 = (char *)malloc(strlen(argv[1])+1);

    strcpy(iname, argv[1]);
    strcpy(iname2, iname);

    // MPI Setup

    int rank, size, len;
    char name[MPI_MAX_PROCESSOR_NAME];

    MPI_Init(&argc, &argv);

    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    MPI_Get_processor_name(name, &len);

    // Create message from rank 0 to broadcast to all processes.

    strcat(iname, "@");
    strcat(iname, name);

    int inameSize = strlen(iname);

    // Create buffer for message

    char* buff = (char *)malloc(inameSize);

    // Sending process fills the buffer

    if (rank == 0)
    {
      strcpy(buff, iname);
    }

    // Send the message
    
    MPI_Bcast(buff, inameSize, MPI_CHAR, 0, MPI_COMM_WORLD);

    MPI_Barrier(MPI_COMM_WORLD);

    // Send different messages from different ranks

    // Send hello from rank 0

    if (rank == 0)
    {
      printf("Hello world, my name is %s, I am sending this message from process %d of %d total processes executing, which is running on node %s. \n", iname2, rank, size, name);
    }

    // Send responce from the other ranks

    if (rank != 0)
    {
      printf("Hello, %s I am process %d of %d total processes executing and I am running on node %s.\n", buff, rank, size, name);
    }

    MPI_Barrier(MPI_COMM_WORLD);

    MPI_Finalize();

    return 0;
  }

```

To try this example yourself you will first need to compile the example code.

If the file that contains the above code is called `helloWorldMPI.c` then to compile on {{ machine_name }} use command,

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise0/Hello-MPI-Compile.md\n```'.replace("REPLACE",machine_name) }}

We can run this executable using this bash script,

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise0/Hello-MPI-SlurmA.md\n```'.replace("REPLACE",machine_name) }}

In the above script we assume one process per node this means we see one process per node responding to our welcome.

Example output:

```
Hello world, my name is your-name, I am sending this message from process 0 of 4 total processes executing, which is running on node nid001059.
Hello, your-name@nid001059 I am process 1 of 4 total processes executing and I am running on node nid001060.
Hello, your-name@nid001059 I am process 2 of 4 total processes executing and I am running on node nid001069.
Hello, your-name@nid001059 I am process 3 of 4 total processes executing and I am running on node nid001098.
```

We can however have multiple processes per node, if we update our bash script to,

{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise0/Hello-MPI-SlurmB.md\n```'.replace("REPLACE",machine_name) }}

In this updated script we have increased the number of processes per node. This means we see multiple processes respond per node.

```
Hello world, my name is yourname, I am sending this message from process 0 of 8 total processes executing, which is running on node nid001452.
Hello, your-name@nid001452 I am process 1 of 8 total processes executing and I am running on node nid001452.
Hello, your-name@nid001452 I am process 2 of 8 total processes executing and I am running on node nid001452.
Hello, your-name@nid001452 I am process 3 of 8 total processes executing and I am running on node nid001452.
Hello, your-name@nid001452 I am process 4 of 8 total processes executing and I am running on node nid001453.
Hello, your-name@nid001452 I am process 5 of 8 total processes executing and I am running on node nid001453.
Hello, your-name@nid001452 I am process 6 of 8 total processes executing and I am running on node nid001453.
Hello, your-name@nid001452 I am process 7 of 8 total processes executing and I am running on node nid001453.
```

## Hybrid

This hybrid code has all the process respond to an initial message from process 0. As this is a hybrid code each of the threads in each process respond with a message.  

```
#include <stdio.h>
#include <mpi.h>
#include <omp.h>
#include <iostream>
#include <string.h>

int main(int argc, char *argv[])
{
    // Check input argument

    if(argc != 2)
    {
        printf("Required one argument `name`.\n");
        return 1;
    }

    // Receive arguments

    char* iname = (char *)malloc(strlen(argv[1])+1);
    char* iname2 = (char *)malloc(strlen(argv[1])+1);

    strcpy(iname,argv[1]);
    strcpy(iname2, iname);

    // MPI Setup

    int rank, size, len;
    char name[MPI_MAX_PROCESSOR_NAME];

    MPI_Init(&argc, &argv);

    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    MPI_Get_processor_name(name, &len);

    // Create message to broadcast to all processes.

    strcat(iname, "@");
    strcat(iname,name);

    int inameSize = strlen(iname);

    // Create buffer for message

    char* buff = (char *)malloc(inameSize);


    // Sending process fills the buffer
      if (rank == 0)
    {
      strcpy(buff, iname);
    }

    // Send the message

    MPI_Bcast(buff, inameSize, MPI_CHAR, 0, MPI_COMM_WORLD);

    MPI_Barrier(MPI_COMM_WORLD);

    if (rank == 0)
    {
      printf("Hello world, my name is %s, I am sending this message from process %d of %d total processes executing, which is running on node %s. \n", iname2, rank, size, name);
    }

    if (rank != 0)
    {
      #pragma omp parallel
      {
        printf("Hello, %s I am thread %d of %d threads in process %d of %d total processes executing and I am running on node %s.\n", buff, omp_get_thread_num(), omp_get_num_threads(), rank, size, name);
      }
    }

    MPI_Barrier(MPI_COMM_WORLD);

    MPI_Finalize();

    return 0;
  }
```

The bash script to run this for {{ machine_name }} ,


{{  '```{include} ../../substitutions/substitutions_REPLACE/Exercise0/Hello-HYB-Slurm.md\n```'.replace("REPLACE",machine_name) }}

In this example has the 0th process sends the a message your-name@nodeId to all the other processes and the print a message in a response.  

```
Hello world, my name is your-name, I am sending this message from process 0 of 8 total processes executing, which is running on node nid001780.
Hello, your-name@nid001780 I am thread 0 of 2 threads in process 1 of 8 total processes executing and I am running on node nid001780.
Hello, your-name@nid001780 I am thread 0 of 2 threads in process 2 of 8 total processes executing and I am running on node nid001782.
Hello, your-name@nid001780 I am thread 0 of 2 threads in process 3 of 8 total processes executing and I am running on node nid001782.
Hello, your-name@nid001780 I am thread 0 of 2 threads in process 4 of 8 total processes executing and I am running on node nid001783.
Hello, your-name@nid001780 I am thread 0 of 2 threads in process 5 of 8 total processes executing and I am running on node nid001783.
Hello, your-name@nid001780 I am thread 0 of 2 threads in process 6 of 8 total processes executing and I am running on node nid001785.
Hello, your-name@nid001780 I am thread 0 of 2 threads in process 7 of 8 total processes executing and I am running on node nid001785.
Hello, your-name@nid001780 I am thread 1 of 2 threads in process 1 of 8 total processes executing and I am running on node nid001780.
Hello, your-name@nid001780 I am thread 1 of 2 threads in process 2 of 8 total processes executing and I am running on node nid001782.
Hello, your-name@nid001780 I am thread 1 of 2 threads in process 3 of 8 total processes executing and I am running on node nid001782.
Hello, your-name@nid001780 I am thread 1 of 2 threads in process 4 of 8 total processes executing and I am running on node nid001783.
Hello, your-name@nid001780 I am thread 1 of 2 threads in process 5 of 8 total processes executing and I am running on node nid001783.
Hello, your-name@nid001780 I am thread 1 of 2 threads in process 6 of 8 total processes executing and I am running on node nid001785.
Hello, your-name@nid001780 I am thread 1 of 2 threads in process 7 of 8 total processes executing and I am running on node nid001785.
```

This uses a simple broadcast for this example but it illustrates that a message has been passed to all the other nodes and each thread is able to access the transmitted information and write a custom response.

## Conclusion

The point of this exercise was to show that there are different ways to parallelise a program and use the hardware a high performance computer gives you access to. The examples here just report the location of each process and thread however in a more realistic scenario each of these examples are different ways to organise a calculation. We might choose different ways to spread out our calculation based on the memory requirement, processing power and communication strategies that are optimal for a given simulation. Choosing the correct strategy can give performance benefits but potentially at the cost of more complex code.

A few other tests you can try to solidify your knowledge:

- Run the serial code with more than one process what do you see?
- Running the threaded code with more than one process what do you see?
- running the MPI code on a single node with multiple processes what you you see?

Remember that message passing codes cost an overhead to send messages and threaded codes cant scale beyond the size of a single node.
