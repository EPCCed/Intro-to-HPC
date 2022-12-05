# Discovering new materials

```{figure} ./images/hero_00a565ef-c294-4b52-902c-f904048bb8ad.jpg

Photo-excitation dynamics in organic solar cells.

© 2016 ARCHER image competition

```

## Materials Science

We live in an age of engineered materials. New batteries, fuel and solar cells, computer chips, and fiber-optic glass are just a few examples of materials that changed our every-day lives. Yet designing new materials has never been easy and usually involved a lot of guesswork and accidental discoveries. Using the power of supercomputers to solve the equations of quantum mechanics allows new technologies to be developed more efficiently.

From the socio-economic perspective, the most important aspect of new materials is their usability. The usability of a material, and hence its engineering application, is determined by its properties, which in turn depend on its internal structure. In other words, the underlying aim of materials science is to understand the relation between the structure of materials and their properties. To fully understand this, it is necessary to understand how different atoms, ions and molecules are arranged and bonded to each other, and this requires the use of quantum mechanics.

Quantum mechanics is the theory on which the behavior of atoms and molecules is based. Material physics has to use quantum theory in systems that contain a huge number of atoms and/or molecules. This makes it extremely difficult to derive the properties of matter. Despite the adoption of approximations and theoretical methods, studying the electronic structure of materials is impossible without the help of computer simulations.

Researchers use specialised software such as Quantum ESPRESSO in conducting quantum materials research. [Quantum ESPRESSO](http://www.quantum-espresso.org/) is an integrated suite of computer codes for electronic-structure calculations and materials modelling. Its intensive use of dense mathematical routines makes it an ideal candidate for many-core architectures, such as the Intel Xeon Phi processor.

Together with the theoretical approach and the experimental studies, why do you think computational studies are a key to studying fundamental properties of the physics and chemistry of materials?

© CINECA

---

## Why use supercomputers in materials science? 

### Video

Discovering_new_materials_hd

```{solution} Transcript

0:11 - My name is Arrigo Calzolari. I am a researcher at the Instituto of Nanoscienze of the National Research Council of Italy. And I just talk about why simulations with supercomputers are so important in our research activity. When you talk about simulation of materials, actually, you are dealing with a very broad area of interest. You are dealing with different kind of system, different kind of chemical composition, different kind of morphologies, from bulk to surfaces or interfaces, but also nano structures, so different kind of dimensionalities and different kind of properties that you are potentially interested in, so electronic properties, optical ones, mechanical properties, and so on and so forth.

1:14 - So when we are talking about simulation of material, we have to keep in mind that what we would like to study is the complexity of material system. And in particular, we would like to understand both their intrinsic properties but also the interaction with this material with the rest of the world. That means that so we would like to see investigate how they’ll react when they are subjected to external interaction, for example with light, or with a mechanical stress, and so on and so forth. The complexity of the real world and the complexity of the system that now experimentally are accessible force us to use even more complex system.

2:14 - And thus, also the simulation that we need to investigate these materials are even more complex. And this is why the use of supercomputers are so important, because typically, so the realistic system require the use of simulation techniques that are very sophisticated. That means that we need very huge resources to describe big system, but also can be necessary to use complicated algorithm to describe complicated properties, for instance, I don’t know, so the transport properties in solids or the optical properties in other structures. So simulation can help, because simulation can give us– so, first of all, they allow us to control the system, to have a perfect control on the geometry, for instance, or the chemical composition of a material.

3:29 - But also, it allows us to search for new material that is, for instance, what happens in high throughput research for material genomics. So the use of simulation and the use, in particular, of supercomputers to perform advanced simulation is a necessary tool to have a deep insight on matter and interaction with matter, and alike, just an example, just to elucidate what we can learn. So, of course, if the system is simple and you are interested in very generic properties, all these complicated machineries typically are not used.

4:26 - But if you want to understand, for instance, what is the relationship between a material and interfaces and what is the behaviour of a material with the rest of the environment, in this case, it’s absolutely necessary to understand what happens, for instance, at the external layer, what is the effect of the formation of the breaking of chemical bonds, for instance, at the interfaces. In all these cases, it’s really necessary to understand what happened, specifically and locally, in this kind of situation.

```

In this video, Dr. Arrigo Calzolari talks about the role of supercomputers in discovering new materials.

Having watched the video, what do you think are the main reasons for using supercomputers to study the properties of new materials?

© CINECA

---

```{figure} ./images/hero_2b49ff3b-bdca-487e-9723-dcd51bc649b3.jpg

A flexible display based on Organic Field-Effect Transistor

© RDECOM (http://www.flickr.com/photos/rdecom/4146880795/)

```

## Understanding new materials

One example of how Quantum ESPRESSO is used to study a real device of promising scientific and technological interest is the electrical conductivity of a PDI-FCN2 molecule. The object of the study is a two-terminal device based on a PDI-FCN2, a molecule derived from perylene. This system is important for the study of the electron transport in single-molecule devices and the further development of a new generation of organic field-effect transistor (OFET).

This study was conducted by CINECA, the Italian Supercomputing Center, in collaboration with the University of Bologna and the National Research Council of Italy - Institute for Nanoscience (CNR-NANO).

Carlo Cavazzoni (senior technologist at CINECA) explains the importance of the research:

“Based on the results obtained from this study, we will gain a deep understanding in the intimate conduction mechanisms of this type of organic device, going a step forward in the direction of utilizing the new OFET technologies that will soon replace traditional silicon devices. Quantum ESPRESSO and new supercomputing facilities will make it possible to study and better understand the physics of the devices that, in the future, will be the building blocks for new photovoltaics cells, next-generation displays and molecular computers.”

The simulated system is composed of two gold electrodes, each of them made from 162 gold atoms. Between the electrodes, there is a PDI-FCN2 molecule. The system is made of 390 atoms and 3852 electrons. The metallic nature of the electrodes requires the simulated space to be represented by many points to describe the electronic structure accurately (more precisely, it requires a fine sampling of the [Brillouin Zone](https://en.wikipedia.org/wiki/Brillouin_zone)). This further increases the computational effort required to simulate this system.

```{figure} ./images/hero_85c10ac4-765c-418c-b940-d3bbc138948e.png
```

The use of Quantum ESPRESSO with the supercomputers at CINECA permitted researchers to calculate the electrical transport properties of the PDI-FCN2 molecule, opening the way to new bio-transistors. In order to compute these properties, up to some tens of thousands of cores on the [FERMI supercomputing cluster](http://www.hpc.cineca.it/hardware/fermi) were used.

Quantum ESPRESSO implements the distributed memory model with several levels of parallelisation, in which both calculations and data structures are distributed across CPU-cores. CPU-cores are organized in a hierarchy of groups, corresponding to the structure levels of the studied material. CPU-cores belonging to different communication levels have different communication patterns - lower level groups are more tightly coupled and require more communication. This essentially means that near neighbours talk more to each other than CPU-cores that are further away from each other.

An important aspect we have not discussed is how the MPI processes are assigned to physical CPU-cores. MPI processes can always communicate with each other regardless of their physical location, but clearly the time taken will vary. Given the above communications patterns, how would you assign the MPI processes to CPU-cores? You should consider a standard supercomputer i.e. a network of shared-memory nodes - Wee ARCHIE is a good example.

© CINECA

---

```{figure} ./images/hero_c952b5d9-2328-4b4a-b1b0-9733c5f96b15.jpg
© iStock.com/nikolay100
```

## Benefits of Hybrid Programming (Discussion)

A significant part of this course is about shared and distributed memory models and architectures, and yet both projects we have talked so far may give you an impression that shared memory models are not being used.

That is not completely true. As you have learned the major drawback of shared memory approach is its scale - it’s simply not feasible to run code using more threads than there are cores available. However, it is possible to combine both approaches - using a shared memory model within the nodes and distributed memory model to communicate between the nodes.

Quantum ESPRESSO uses a mixed MPI-OpenMP implementation. The idea is to have one (or more) MPI process(es) per multicore node, with OpenMP parallelisation inside the same node.


```{figure} ./images/hero_5ecd9fac-24c3-435a-ad62-0081447ec565.png
```

Hybrid programming diagram (image)

Carlo Cavazzoni explains the benefits of this approach:

“The availability of a large number of cores per node has made it possible to tune the different layers of parallelization. A good tiling of the different data structures permits us to efficiently use the memory and computing power of each node, reducing the amount of communication and, thus, enhancing the performance. However, there is still room for improvement, in particular for the efficiency of the OpenMP multithreading that is currently limited to 4-8 threads. This is because workloads that are too small can induce load imbalance and leave CPU-cores idle.”

The term load imbalance refers to a situation where the computational work is not distributed equally among the CPU-cores. This causes different CPU-cores to finish their pieces of work at different times, after which they need to wait for other CPU-cores to finish their work.

- What do you think of this approach?
- Do you understand why having too many too small workloads is detrimental to the performance?
- What do you think is better? Using as many threads as there are cores but incurring higher parallel overheads? Or leaving some of the cores idle? Why?
- Can you think of any way to to address this issue?

© CINECA
