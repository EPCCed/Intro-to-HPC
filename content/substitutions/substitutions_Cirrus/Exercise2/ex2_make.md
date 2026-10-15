```bash
cd fractal
```

Then you will need to edit the Makefile so that the ``CC`` variable is set to ``cc`` (the Cray C wrapper which includes MPI support)

``CC = cc``

you will then be able to run the make command

```bash
make
```