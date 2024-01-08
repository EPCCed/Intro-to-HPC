# Simulating the Brain

```{figure} ./images/hero_eb918db9-1d0c-4bd6-addc-14a7aaede9e5.png

Simulation of electrical activity in a “virtual brain slice” formed from seven unitary digital reconstructions of neocortical microcircuits.

© EPFL/BlueBrainProject

```

## The Blue Brain Project

The [Blue Brain Project](http://bluebrain.epfl.ch/page-52063.html) (BBP) aims to build comprehensive digital reconstructions (computer models) of the brain, which include the brain’s different levels of organization and their interactions, and which are compatible with the available experimental data. The supercomputer-based reconstructions and simulations built by the project offer a radically new approach for understanding the multilevel structure and function of the brain.

With its 100 billion neurons (brain cells) and its 100 trillion synapses (biological pathways through which signals can be exchanged), the human brain is a complex multi-level system. The connections among the neurons form a hierarchy of circuits, from local microcircuitry up to the level of the whole brain. Meanwhile, at a lower level, every neuron and every synapse is a complex molecular machine in its own right. It is the interactions between these levels, which give rise to human behaviour, human emotion and human cognition.

The BBP’s reconstruction strategy identifies interdependencies in the experimental data and uses them to constrain the reconstruction process. Multiple, intersecting constraints allow the project to build the most faithful reconstructions possible from the sparse available experimental data – avoiding the need to measure everything.

In fact, the BBP’s latest reconstruction of the cortical microcircuit connectivity is based on experimental data representing less than 1% of the synaptic connections in the circuit. Lab experiments providing new data and constraints make it possible to test and progressively improve the digital reconstruction. Data and neuron models used in the current reconstructions are already available at the project’s [NeuroMicrocuitry Web Portal](https://bbp.epfl.ch/nmc-portal) and will soon be accessible through the [Human Brain Project’s Brain Simulation Platform](https://www.humanbrainproject.eu/en/brain-simulation/brain-simulation-platform/).

Simulations give insight into experiments that would be difficult or impossible in biological tissue or in living animals – for instance, experiments requiring simultaneous measurements from large numbers of neurons. Such experiments provide a new tool for researchers seeking to understand the causal relationships between events at different levels of brain organisation.

The supercomputer-based simulations turn understanding the brain into a tractable problem, providing a new tool to study the complex interactions within different levels of brain organization and to investigate the cross-level links leading from genes to cognition.

In your opinion, what are the implications of being able to study certain aspects of the brain without the need to measure everything?

© EPFL/BlueBrainProject

---

```{figure} ./images/hero_5cab4b79-a4ad-4a36-9052-d530f82ba4ee.jpg

An image of a layer 2/3 large basket cell in a digital reconstruction of neocortical microcircuitry.

© EPFL/BlueBrainProject

```
## Reconstruction and Simulation of Neocortical Microcircuitry

One of the BBP’s achievements was the digital reconstruction and simulation of the neocortical microcircuitry of a juvenile rat. The reconstruction uses cellular and synaptic organizing principles to algorithmically reconstruct detailed anatomy and physiology from sparse experimental data.

The neocortex, being responsible for higher order brain functions such as sensory perception, spatial reasoning, cognition and language, is the most developed part of the brain in its organisation and number of layers.

The simulated neocortical volume contains ∼31,000 neurons and 55 layer-specific morphological and 207 morpho-electrical neuron subtypes. When digitally reconstructed, neurons are positioned in the volume and synapse formation is restricted to biological bouton (a part of a synapse) densities and numbers of synapses per connection. Their overlapping arbors form ~8 million connections with ~37 million synapses.

The phrase without the need to measure everything used in the previous step, in this case, refers to the fact that the number of synapses and connections, and the connectivity between neurons, is not based on a particular set of experimental data. The whole network is reconstructed based on probabilities of the connections occurring.

If you are interested in knowing why this approach works, watch Prof. Henry Markram explain [how neurons connect to each other](https://www.youtube.com/watch?v=ySgmZOTkQA8&t=190s).

To learn more about the study, see the [Reconstruction and Simulation of Neocortical Microcircuitry video](https://www.youtube.com/watch?v=IL08fnRCb0k&list=PLHyCyen_OSEPIptWN2VWlruGMi9nk2-3i).

If you would like to see more visualisations from the project, see the [Blue Brain Project Movie](https://www.youtube.com/watch?v=0TMeRdDEuIg).

Now you should have a rough idea what these simulations are all about. Can you imagine how they are parallelised? Why do you think so?

© EPFL/BlueBrainProject

---

```{figure} ./images/hero_65a95499-77da-4b7f-a6e3-6efc591e01e1.png

In silico retrograde staining. The presynaptic neurons of a layer 2/3 nest basket cell (in red) were stained (in blue) in a digital reconstruction of neocortical microcircuitry. Only immediate neighbouring presynaptic neurons are shown.

© EPFL/BlueBrainProject

```

## NEURON simulation environment

The Blue Brain workflow creates enormous demands for computational power. In Blue Brain cellular level models, the representation of the detailed electrophysiology and communication of a single neuron can require as many as 20,000 differential equations. Even modern multi-core workstations have difficulties in solving this number of equations in biological real time. In other words, the only way to ensure reasonable turn-around times for simulating networks of neurons is the use of High Performance Computing (HPC).

The primary software used by the BBP for neural simulations is a package called [NEURON](https://www.neuron.yale.edu/neuron/). It’s a powerful tool used for constructing, managing, and exercising biologically realistic neuronal models. NEURON was extended from a single CPU to multiple CPUs to support complex computational model simulation. It can run parallel simulations on small clusters with 10–50 processors, up to large scale supercomputers with thousands of processors.

### Distributing the Computation

Neurons communicate through [action potentials](https://en.wikipedia.org/wiki/Action_potential), also called nerve impulses or spikes. Usually, each neuron sends to thousands of other neurons and receives from thousands of neurons. Can you imagine the complexity of the network they create? To simulate this network in a distributed memory environment, it has to be divided into smaller pieces.

In other words, we want each CPU-core to execute exactly the same program but on a different subset of neurons. The problem is that source cells (neurons sending the impulses) and their targets (neurons receiving them) usually are not on the same CPU-cores.

The solution adopted is to assign a global identifier to all cells regardless of which CPU-core’s memory they reside in, and allow all CPU-cores to keep track of all spikes. Because the simulation takes a synapse-centric view of the network (it’s more about a message and its path rather than its sender or receiver), these identifiers, with appropriate weights and delays, are used to propagate the nerve impulses between the brain cells.

### Parallel Model Execution

There is no master process (the boss telling other processes what to do and when) so every process integrates the equations for its subnet over an interval equal to the minimum time needed for cells to be stimulated.

Then the processes use MPI communication to exchange data with each other so that the spike times are broadcast to all processes in one global communication step. At the end of the step, every process has access to all the spike times computed by every other process. This requires both sender and receiver to participate in the communication process explicitly and requires extra synchronisation among processes.

### Scaling

The problem with this approach is that increasing the number of processes also increases the communication overheads. The interprocessor spike exchange consumes a large fraction of the overall simulation time.

Can you explain why this behaviour is observed? What do you think this means for the performance of the simulation? Do you have any ideas how to improve this situation?

© EPFL/BlueBrainProject

---

```{figure} ./images/hero_e93d190f-b32b-4add-8558-c72bff25d0f7.jpg

© iStock.com/natasaadzic

```

## What's next?

The Blue Brain project’s simulation of the neocortical column incorporates detailed representations of at least 30,000 neurons – in most cases several times this number, in order to provide valid boundary conditions. A simulation of a whole brain rat model at the same level of detail would represent up to 200 million neurons and would require approximately 10,000 times more memory. Simulating the human brain would require yet another 1,000-fold increase in memory and computational power.

Can you imagine a supercomputer capable of running simulations of this scale? How soon do you think it could happen?

In 2013 the Blue Brain project published the following roadmap:

```{figure} ./images/hero_be811f55-d07e-4d0d-83e1-82c15399853c.jpg
```

What do you think about this Roadmap? Remember that the project’s main publication (mentioned in one of the videos) titled the Reconstruction and Simulation of Neocortical Microcircuitry was published in October 2015.

Taking into account recent developments in the supercomputing world, what is your prediction for the future of the Blue Brain Project?


© EPFL/BlueBrainProject
