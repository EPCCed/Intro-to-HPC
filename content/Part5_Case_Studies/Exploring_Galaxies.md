# Exploring Galaxies

```{figure} ./images/hero_8d95d1ba-c581-42ac-98c1-a35652031c69.png

Stellar light distribution of the most massive cluster in the simulation volume.
© 2015 The Illustris Collaboration.

```


## Illustris Project

The Illustris project is a large cosmological simulation of galaxy formation, completed in late 2013, using a state of the art numerical code and a comprehensive physical model. Building on several years of effort by members of the collaboration, the Illustris simulation represents an unprecedented combination of high resolution, total volume, and physical fidelity.

The standard model of cosmology posits that the mass-energy density of the Universe is dominated by unknown forms of dark matter and dark energy. Testing this extraordinary scenario requires precise predictions for the formation of structure in the visible matter, which is directly observable as stars, diffuse gas, and accreting black holes. These components of the visible matter are organized in a ‘Cosmic Web’ of sheets, filaments, and voids, inside which the basic units of cosmic structure - galaxies - are embedded.

To test their current ideas on the formation and evolution of galaxies, cosmologists strive to create simulated galaxies as detailed and realistic as possible, and compare them to galaxies observed in the real universe. By probing their successes and failures, they can further enhance their understanding of the galaxy formation process, and thereby perhaps realize something fundamental about the world in which we live.

The Illustris project is a set of large-scale cosmological simulations, including the most ambitious simulation of galaxy formation yet performed. The calculation tracks the expansion of the universe, the gravitational pull of matter onto itself, the motion or “hydrodynamics” of cosmic gas, as well as the formation of stars and black holes. These physical components and processes are all modeled starting from initial conditions resembling the very young universe 300,000 years after the Big Bang until the present day, spanning over 13.8 billion years of cosmic evolution.

The simulated volume contains tens of thousands of galaxies captured in high-detail, covering a wide range of masses, rates of star formation, shapes, sizes, and with properties that agree well with the galaxy population observed in the real universe. They are currently working to make detailed comparisons of our simulation box to these observed galaxy populations, and some exciting promising results have already been published.

The Illustris project has had a lot of media coverage. Below is a copy of the project poster - for more details visit the Illustris [Press page](http://www.illustris-project.org/press/).

```{figure} ./images/hero_66db8146-b6cb-4133-92e7-1d733a6f6fe9.jpg
```


Illustris poster © 2015 The Illustris Collaboration (image)

---

## The most detailed simulation of our Universe

```{danger}
need to source video
```

The above video is a compilation made from some of the movies available on the project’s [Media page](http://www.illustris-project.org/media/), where many additional visualisations are available.

In addition to gravity and hydrodynamics, very complex physical processes such as chemical processes in the diffuse gas, radiation, and magnetic fields also affect cosmic structure formation. Moreover, structure formation is a self-regulating process, in the sense that the structures that form, in particular stars and black holes, affect their surroundings and the subsequent evolution of the next generation of structures.

In Illustris, a comprehensive (even if not complete) set of physical processes such as star-formation-driven galactic winds, and black hole thermal energy injection, are modeled throughout cosmic history. These sophisticated models are crucial for achieving a realistic population of modeled galaxies.

Do you have a better idea of the scale and complexity of this simulation now? What do you think? How do you think it could be parallelised?

© 2015 The Illustris Collaboration

---

```{figure} ./images/hero_b499eb13-3422-4bb6-8de1-803848665cd8.png

Dark matter density overlaid with the gas velocity field.

© 2015 The Illustris Collaboration.

```

## About the Simulation

It’s time to have a closer look at the computational methods and parallelisation strategy used in the Illustris simulation.

Don’t worry if you don’t quite understand all the terms used, it’s enough for you to have a general understanding of the complexity involved.

### Computational methods

Over the past few decades, computer simulations of the evolution of the universe, including only the effects of gravity, have reached a point of maturity and accuracy, the [Millennium simulation](http://wwwmpa.mpa-garching.mpg.de/galform/virgo/millennium/) standing as a prime example. Those which attempt to also include a treatment for gas, such as Illustris, have proven to be significantly harder. A number of fundamentally different methods exist for simulating gas on a computer. In astrophysics, most researchers have used one of the two approaches:

1) The first one is smoothed particle hydrodynamics, or SPH, where the mass of the gaseous fluid is parceled out to a discrete number of particles. These particles move in response to the combined forces of gravity and hydrodynamics, and their position at any time indicates where the gas is, and how it is moving.
2)The second approach is to use a Eulerian or mesh-based methods, typically utilizing a scheme called “adaptive mesh refinement” or AMR. In this method, space itself is divided up into a grid, and the flow of gas between neighboring cells of this grid is computed over time.

The Illustris simulation uses a different approach, as implemented in the [AREPO code](http://wwwmpa.mpa-garching.mpg.de/~volker/arepo/), which we typically refer to as employing a moving, unstructured mesh. Like in AMR, the volume of space is discretized into many individual cells, but as in SPH, these cells move with time, adapting to the flow of gas in their vicinity. As a result, the mesh itself, called a Voronoi tessellation of space, has no preferred directions or grid-like structure. Over the past few years we have shown that this new type of approach for simulating gas has significant advantages over the other two methods, particularly for large, cosmological simulations like Illustris.

### Parallelisation Strategy

Parallelisation of simulation codes for distributed architectures requires decomposition of a problem into individual parts and to ensure scalability data duplication should be avoided.

The approach taken in the Illustris simulation is based on decomposing the point set into disjoint spatial domains, each mapped to a different CPU-core with its own physical memory. The idea here is that most of the cells of a domain will lie in its interior and hence only depend on the data local to the CPU-core, while some cells close to the surface may be affected by data on other CPU-cores, which needs to be dealt with by data communication. The strategy to deal with this issue is to construct a locally complete mesh by importing ghost points (points that belong to other CPU-cores but need to be communicated for the calculation to be correct) from neighbouring CPU-cores so that all the required data is available locally.

These ghost points provide the glue that gives the proper connectivity across domains. They are also used to implement periodic or reflecting boundary conditions, which are simply realized through fiducial points that are imported from the other side of the simulation box. In practice, the use of ghost points increases the number of cells that need to be considered by a given CPU-cores typically by 3 − 10%, depending on how many CPU-cores are used. This induces significant overhead and limits the scalability but only when small problems are run on a large number of CPU-cores.

### Supercomputing resources 

In addition to being accurate, the AREPO code is also efficient – it can run on tens of thousands of computer cores simultaneously, leveraging some of the largest computers that currently exist for scientific research within the high performance computing (HPC) community.

The Illustris simulations were run on the following supercomputers:

- the CURIE supercomputer at CEA, France as part of PRACE project
- the SuperMUC computer at the Leibniz Computing Centre, Germany
- the Odyssey and CfA/ITC clusters at the Harvard University, USA
- the Ranger and Stampede supercomputers at the Texas Advanced Computing Center, USA
- and the Kraken supercomputer at Oak Ridge National Laboratory, USA.

The largest simulation was run on 8,192 compute cores, and took 19 million CPU hours - the equivalent of one computer CPU running for 19 million hours, or about 2,000 years!

© 2015 The Illustris Collaboration

---

```{figure} ./images/hero_92e9ab13-b781-4264-8b68-b6143b90d023.jpg
© iStock.com/AndreyPopov
```

## Communication Costs

In the last step, you have learned that each CPU-core is responsible for a volume of space and all physical processes occurring inside it. Naturally, the physical laws do not care about our artificial division of space, and effects such as short-range gravitational field or zone of accretion around a supermassive black hole, may affect the space belonging to multiple CPU-cores.

However, because there is no fixed, permanent grid-like structure in the simulated space, the communication patterns between the CPU-cores cannot be predicted ahead of time.

Therefore, to calculate these spilled over effects, each MPI process does a [range-search](https://en.wikipedia.org/wiki/Range_searching) (a check to determine if an object is in an influenced area) on its local domain. At the same time, it also detects if the search region overlaps with other processes’ domains, and, if so, registers the ID of these processes in a table. After this search, the table is communicated with an MPI function, which sends data from every process to every other process, so that each MPI process knows how many items it needs to import from each of the other processes. This global communication pattern is called all-to-all.

Notice that using the dynamic unstructured mesh technique to divide the simulated space amongst the processes means that most of the calculations are local. What does that mean? It means that a large number of the messages exchanged through the MPI all-to-all function have zero entries i.e. the MPI processes don’t really need to talk to each other. This is an area which could be made more efficient.

Recall what you have learned about the effect of bandwidth and latency on the speed of supercomputers. What do you think that means for the code performance? Why does it become an issue in simulations with more than 10,000 MPI processes? Do you have any ideas how to fix this?

© 2015 The Illustris Collaboration


