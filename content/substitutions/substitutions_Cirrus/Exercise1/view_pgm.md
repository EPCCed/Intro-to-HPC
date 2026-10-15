To view images on Cirrus you can use Python's built-in tkinter (no extra modules needed):
``` bash
    module load cray-python
    python -c "import tkinter as tk; r=tk.Tk(); img=tk.PhotoImage(file='img0001.pgm'); tk.Label(r, image=img).pack(); r.mainloop()"
```

This requires **X11 forwarding** to be enabled — connect with `ssh -Y username@login.cirrus.ac.uk` (or `ssh -X` on older systems) to forward the display to your local machine.

To avoid retyping the one-liner, create a small script (e.g. `viewer.py`):

``` python
#!/usr/bin/env python
import tkinter as tk
import sys

f = sys.argv[1] if len(sys.argv) > 1 else 'img0001.pgm'
r = tk.Tk()
r.title(f)
img = tk.PhotoImage(file=f)
tk.Label(r, image=img).pack()
r.mainloop()
```

Then run `python viewer.py img0001.pgm` (or just `python viewer.py` for the default filename).

```{Note}
Here we have introduced the ``module`` command, this is a way of controlling the software environment typically used on HPC machines. A module is a self-contained description of a software package -- it contains the settings required to run a software package and, usually, encodes required dependencies on other software packages.
```