# Part 2: Investigation

## Run tests

To explore the effect of the load balancing run the code with different number of workers and tasks and try to answer the following questions:

- From the default run with 16 workers and 16 tasks, what is your predicted best runtime based on the load imbalance factor?

- Look at the output for 16 tasks – can you understand how the load was distributed across workers by looking at the colours of the bands and the structure of the Mandelbrot set?

- Increase the number of tasks by decreasing the task size; does the runtime approach what you predicted? Look at the colour bands: how does the task distribution change? Are the results qualitatively different if you use more workers?

- For 16 workers, run the program with ever smaller task sizes (i.e. more tasks) and plot a graph of runtime against number of tasks. You should ensure you measure all the way up to the maximum number of tasks, i.e. a task size of a single pixel.

- Can you explain the form of the graph? Does the minimum runtime approach what you predicted from the load imbalance factor?

- You can experiment with varying the parameters, e.g. plotting the same graph with more workers or using the Julia set rather than the Mandelbrot set.


The number of workers is controlled by the ``–n`` argument to ``srun`` and is one fewer than P (the total number of processes) as we always require one dedicated process to be the controller.