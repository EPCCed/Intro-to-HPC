```bash
cd fractal
```

You will first need to load the ``mpt`` module (Message Passing Toolkit) which contains the MPI implementation

```
module load mpt
```
Then you will need to edit the Makefile so that the ``CC`` variable is set to ``mpicc``

``CC = mpicc ``

you will then be able to run the make command

```bash
make
```