# Connecting Multiple Computers

## Distributed Memory Architecture

Because of the difficulty of having very large numbers of CPU-cores in a single shared-memory computer, all of today’s supercomputers use the same basic approach to build a very large system: take lots of separate computers and connect them together with a fast network.

For the moment, let’s ignore the complication that each computer is itself a shared-memory computer, and just say we have one processor in each computer:

Diagram of distributed memory system (image)

The most important points are:

every separate computer is usually called a node
each node has its own memory, totally separate from all the other nodes
each node runs a separate copy of the operating system
the only way that two nodes can interact with each other is by communication over the network.
The office analogy is very useful here: a distributed-memory parallel computer is like workers all in separate offices, each with their own personal whiteboard, who can only communicate by phoning each other.

Advantages	Disadvantages
the number of whiteboards (i.e. the total memory) grows as we add more offices	if we have large amounts of data, we have to decide how to split it up across all the different offices
there is no overcrowding so every worker has easy access to a whiteboard	we need to have lots of separate copies of the operating system
we can, in principle, add as many workers as we want provided the telephone network can cope.	it is more difficult to communicate with each other as you cannot see each others whiteboards so you have to make a phone call
(table)

The second disadvantage can be a pain when we do software updates - we have to upgrade thousands of copies of the OS! However, it doesn’t have any direct cost implications as almost all supercomputers use some version of the Linux OS which is free.

It turns out that it is much easier to build networks that can connect large numbers of computers together than it is to have large numbers of CPU-cores in a single shared-memory computer. This means it is relatively straightforward to build very large supercomputers - it is an engineering challenge, but a challenge that the computer engineers seem to be very good at tackling!

So, if building a large distributed-memory supercomputer is relatively straightforward then we’ve cracked the problem?

Well, unfortunately not. The compromises we have had to make (many separate computers each with their own private memory) mean that the difficulties are now transferred to the software side. Having built a supercomputer, we now have to write a program that can take advantage of all those thousands of CPU-cores and this can be quite challenging in the distributed-memory model.

Why do you think the distributed memory architecture is common in supercomputing but is not used in your laptop?

## Simple Parallel Calculation (Discussion)

Let’s return to the income calculation example. This time we’ll be a bit more ambitious and try and add up 800 salaries rather than 80. This list of salaries fills 8 whiteboards (100 on each) all in separate offices.

Here we are exploiting the fact that distributed-memory architectures allow us to have a large amount of memory.

If we have one worker per office, think about how you could get them all to cooperate to add up all the salaries. Consider two cases:

only one boss worker needs to know the final result;
all the workers need to know the final result.
To minimise the communication-related costs, try to make as few phone calls as possible.

## Case study of a real machine

To help you understand the general concepts we have introduced this week, we’ll now look at a specific supercomputer. I’m going to use the UK National Supercomputer, ARCHER, as a concrete example. As well as being a machine I’m very familiar with, it has a relatively straightforward construction and is therefore a good illustration of supercomputer hardware in general.

### General
ARCHER is a Cray XC30 machine, built by the American supercomputer company Cray Inc. It contains 118,080 CPU-cores and has a theoretical peak performance of 2.55 Pflop/s. It is operated by EPCC at the University of Edinburgh on behalf of EPSRC and NERC, and is the major HPC resource for UK research in engineering and in physical and environmental science.

Note that ARCHER is due to be replaced by ARCHER2 built by Cray, a Hewlett Packard Enterprise company. ARCHER2 will be operated and supported by EPCC at the University of Edinburgh on behalf of UKRI. For full details, see the ARCHER2 web site.

### Node design
The basic processor used in ARCHER is the Intel E5-2697 v2 (Ivy Bridge) CPU, which has a clock speed of 2.7 GHz. The nodes on ARCHER have 24 CPU-cores; all 24 cores in a node are under the control of a single operating system. The OS is the Cray Linux Environment, which is a specialised version of SUSE Linux tailored for Cray systems.

### Network
The complete ARCHER system contains 4,920 nodes, i.e. ARCHER is effectively around 5,000 separate computers each running their own copy of Linux. They are connected by the Cray Aries network, which has a complicated hierarchical structure specifically designed for supercomputing applications. The entire network can support aggregate data transfer rates of over 10 TByte/s (the so-called bisection bandwidth). To put this in context, this is almost a million times faster than what is possible over a 100 Mb fast broadband connection!

### System performance
ARCHER has a total of 118,080 CPU-cores: 4,920 nodes each with 24 CPU-cores. With a clock frequency of 2.7 GHz, the CPU-cores can operate at 2.7 billion instructions per second. However, on a modern processor, a single instruction can perform more than one floating-point operation.

For example, on ARCHER one instruction can perform up to four separate additions. In fact, the cores have separate units for doing additions and for doing multiplications that run in parallel. With the wind in the right direction and everything going to plan, a core can therefore perform 8 floating-point operations per cycle: four additions and four multiplications.

This gives a peak performance of 118,080 * 2.7 * 8 Gflop/s = 2550528 Gflop/s, agreeing with the 2.55 Pflop/s figure in the top500 list.

ARCHER comprises 26 separate cabinets, each about the height and width of a standard door, with around 4,500 CPU-cores (188 nodes) or about 9000 virtual cores (using multi-threading) in each cabinet.

ARCHER's cabinets (image)

### Storage
Most of the ARCHER nodes have 64 GByte of memory (some have 128 GByte), giving a total memory in excess of 300 Tbyte of RAM.

Disk storage systems are quite complicated, but they follow the same basic approach as supercomputers themselves: connect many standard units together to create a much more powerful parallel system. ARCHER has over a thousand 4 Tbyte disks, giving total disk storage in excess of 4 Pbyte.

### Power and Cooling
If all the CPU-cores are fully loaded, ARCHER requires in excess of 2 Megwatts of power, roughly equivalent to a small town of around 2000 houses.

The ARCHER cabinets are kept cool by pumping water through cooling pipes. The water enters at approximately 18°C and, after passing through the cabinets, comes out at around 29°C; it can then be cooled back down and re-circulated. When necessary the water is cooled by electrical chillers but, most of the time, ARCHER can take advantage of the mild Scottish climate and cool the water for free simply by pumping it through external cooling towers, so saving significant amounts of energy.

Diagram of ARCHER's cooling system © Mike Brown (image)

ARCHER's cooling towers © Mike Brown (image)

## Wee ARCHIE case study

### Video

Wee_Archie_case_study_hd

### Transcript

0:11 - So in this video we’re going to talk about Wee ARCHIE. So you’ve already seen a promotional video about Wee ARCHIE, but here we’re going to go into a bit more technical detail. Now Wee ARCHIE is a machine that we’ve built at EPCC specifically for outreach events to illustrate how parallel computing, supercomputing works. And what we do is we take it to conferences and workshops and schools to try and explain the basic concepts. But here I’m going to use it as a way of explaining the kinds of things we’ve been learning this week, which is things like distributed memory computing, shared memory computing, and how they’re put together into a real computer.

0:43 - Wee ARCHIE has been built to mirror the way a real supercomputer like ARCHER, our national supercomputer, is built, but it’s a smaller version and it’s also been designed with a perspex case and designed in a way that we can look inside and show you what’s going on. So it’s very useful to illustrate the kinds of concepts that we’ve been talking about this week. So the ways in which Wee ARCHIE mirrors a real supercomputer like ARCHER are that is a distributed memory computer. It comprises a whole bunch of nodes that are connected by a network and we’ll look at them in a bit more detail later on.

1:16 - Each of the nodes is a small shared-memory computer, each of which is running its own copy of the operating system and in this, just like in ARCHER, we’re running lots of copies of Linux. On Wee ARCHIE, we don’t use very high spec processes. That’s for reasons of economy and power consumption. So on Wee ARCHIE, each of the nodes is actually one of these Raspberry Pis. Now, a Raspberry Pi is a very low power and quite cheap processor, and these ones are actually small shared-memory machines. So each Raspberry Pi has actually got four cores on it. It’s a small quad-core, multi-processor machine, a small-shared memory system, running a single copy of Linux.

1:57 - Now we can look in more detail at how Wee ARCHIE is constructed. So if we look down here, are the computational nodes which is a four by four grid here. We have 16 Raspberry Pi boards, each of which, as I said, is running its own copy of Linux, and each of which is a small shared-memory processor with four CPU-cores. See here they are, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16. Again, just like on a real supercomputer, users don’t log onto these computational nodes, they log onto some front-end login system. And up here we have a couple of spare nodes.

2:31 - Hardware-wise, they’re just the same as the main computational nodes, but they’re reserved for the user to log on and do various other activities, compiling code, launching programs, and doing I/O and such like. So again this is a nice analogue of how a real parallel supercomputer works. Just to look in a bit more detail at the nodes of Wee ARCHIE. You will see there are some lights here. And what we’ve done for demonstration purposes is each node of Wee ARCHIE is connected to its own little LED array. You won’t see particularly much going on at the moment, because we aren’t actually running programs on these nodes.

3:01 - Just for information, what they display are things like the network activity on the node, the temperature of the node, and also the activity, how busy each of the four CPU-cores is. The way the networking works on Wee ARCHIE is that each node has a cable coming out of it, a network cable, and here we just use quite standard cheap networking. It’s called ethernet cabling. It’s the kind of thing you might have at home or in an office. And the cables come out and they connect to these switches here.

3:30 - So the way that two nodes on Wee ARCHIE communicate with each other is, if they want to send a message, it goes down the cables into one of the switches and then back up the other cable into the other node. Now we’ve turned Wee ARCHIE around to look at the back to illustrate a bit more how the networking works and a bit more about how it’s all connected together. So if you can see, there are actually two cables coming out of the computational nodes. One of those is actually power, so we’re not particularly concerned about that. The other connection is a network cable.

3:59 - So the way it works, is that the cables for each node come down into one of these switches. And then the switches themselves, are cabled together. And this illustrates, in quite a simple way, how these networks are hierarchical. So for example, two cores on the same node, two CPU-cores in the same shared-memory processor can communicate with each other without going over the network at all. If two nodes are connected to the same switch, they can communicate with each other by sending the signal down and then back out of the same switch.

4:31 - If two nodes are connected to different switches, the message has to go down into one of the switches along to the other switch and then back to the node again. So I mentioned in the articles that the network on ARCHER, the Cray Aries network, is very complicated, has lots of different levels. But even on a very simple machine like Wee ARCHIE, you can see the network has a hierarchical structure. And so that the way that two CPU-cores communicate with each other is different depending on where they are in the machine. Although it’s been primary built as an educational tool, in fact, Wee Archie has a lot of similarities, as I said, with a real supercomputer.

5:06 - And just to reiterate, they’re things like having a distributed-memory architecture of different nodes, each running their own operating system. The nodes are connected by networking. And each node is actualy a small shared-memory computer. The main way in which Wee ARCHIE differs from a real supercomputer, like ARCHER, is really in some of the performance characteristics. So for example, the processors aren’t as fast as you’d find on a real supercomputer, the networking isn’t as fast. The ethernet we have here is a lot slower than the dedicated Aries network we have on ARCHER. And also, of course, the sheer scale.

5:41 - Here we only have 16 nodes, each of which has four CPU-cores, as opposed to thousands of nodes with tens of CPU-cores in them. And so Wee ARCHIE mirrors a real supercomputer such as ARCHER in almost every way, except for just the speed and the scale.

### Text

Finally, Wee ARCHIE makes its appearance again! This video uses Wee ARCHIE to explain the parallel computer architecture concepts introduced in this week.

It is worth emphasising that the physical distance between the nodes does impact their communication time i.e. the further apart they are the longer it takes to send a message between them. Can you think of any reason why this behaviour may be problematic on large machines and any possible workarounds?

As usual, share your thought with your fellow learners!

For anyone interested in how Wee ARCHIE has been put together (and possibly wanting to build their own cluster), we invite you to follow the links from this blog article - Setting up your own Raspberry Pi cluster.

Wee Archie close-up (image)

Wee Archie logo (image)

## ARCHER - it's more complicated...

In the last few steps we have glossed over a few details of the processors and the network.

If you look up the specifications of the Intel E5-2697 v2 processor you will see that it has 12 CPU-cores, whereas the ARCHER nodes have 24 CPU-cores. This is possible because each node actually contains two physical processors. However, they are put together so that both processors share the same memory so, to the user, it basically looks identical to a single 24-core processor. How this is done is illustrated below and is called a Non-Uniform Memory Access (NUMA) architecture.

NUMA architecture (image)

Although every CPU-core can access all the memory regardless of which processor it is located on, this can involve going through an additional memory bus so reading data from another CPU’s memory is slower than reading from your own memory. This hardware arrangement is really a detail that isn’t usually very important - the most important feature is that the 24 CPU-cores form a single shared-memory computer, all under the control of a single operating system.

An ARCHER compute blade (image)

"An ARCHER compute blade - One blade holds four nodes, each node has two processors, and each processor has 12 CPU-cores. The shiny copper heatsinks help keep the processors cool. © ARCHER"

The details of the network are even more complicated with four separate levels ranging from direct connections between the four nodes packaged together on the same blade, up to fibre-optic connections between separate cabinets. If you are interested in the details see the ARCHER website or this more detailed presentation from Cray.

Archer is a Tier-1 system within PRACE, whereas the Cray Piz Daint system in Switzerland is a more powerful Tier-0 system, despite having a similar number of nodes. Can you find out why this is? What are the similarities and differences?

## MARCONI: building a real supercomputer

### Video

MRCONI_build_hd

### Transcript

0:12 - Having taken a look at how Wee ARCHIE was constructed we thought it’d be interesting to look at how a real supercomputer is put together. So here we have some time-lapse photography of the MARCONI system being put in, which is a large Tier-0 system for PRACE, which is located at the CINECA supercomputing centre at Bologna in Italy. And you’ll see here they’re putting in the infrastructure. There’s some fairly heavy engineering going on, welding and such like, to put in all the services which are required. And the main one here is you’re seeing the cooling infrastructure, the pipes which carry the water, which cools the main machine. Next, we can start laying the raised floor which sits above all the pipework.

0:50 - And the supercomputer cabinets will actually sit on top of this raised floor. Now we jump forward quite a bit and you can see the raised floor is practically complete. And what they’re doing now is they’re working on the ceiling. They’re attaching these rows of trays to the ceiling and we’ll see later why these are needed.

1:12 - Now the major infrastructure work is complete and we’re just having a bit of a spruce up, a clean and a tidy, to make sure that the machine room is spotless before the arrival of the main system. Now the system engineers are having various conferences and conversations just to make sure they’re well-prepared, that they all know what they’re doing, before the arrival of the cabinets. Now the cabinets start to arrive. And you can see we take out tiles in the raised floor so we can move the cabinets into the right place and connect them to the infrastructure below.

1:40 - Now we can’t see exactly what’s going on on the right-hand side when the first row goes in– we’ll get a better view of the second row. It’s worth looking at these cabinets. You’ll see that they’re about the size of a door. They’re very deep, but their profile is about the size of a door. And that’s because they have to get through a standard door to get into the machine room. You’ll see as the second row goes in, it goes in very, very quickly. The tiles are taken up, the cabinets are put in place, and they’re all connected up. And there we have a completed second row. Just a little bit more work to the floor on the second row.

2:12 - And then we’re ready for putting the third row in. And here it comes. Just the same procedure, take up a tile, move the cabinet into place, and connect it up. So you’ll see very quickly we have three rows of cabinets comprising the main supercomputer, but you might ask, where’s the networking? Networking a real supercomputer is a lot more work than networking a small toy machine, like Wee ARCHIE. And you’ll see there’s lots of people involved here laying and connecting cables. And we immediately see what these raised trays are for. They’re there to carry the cables between the cabinets to keep them neat and tidy away from the cabinets and keep them so they’re not all tangled up.

2:51 - So we’ve largely finished the cabling, the interconnect, on the second row. Now they’re working on the third row. And you’ll see there a lot of human work here and that is because, although supercomputers are made by a few manufacturers, they’re quite similar to each other, each individual installation is largely bespoke. This MARCONI system, which is delivered by Lenovo, is of a certain class. But it won’t be the same as any other particular machine. It will be slightly different. And so each machine has to be constructed very carefully to make sure it all works together. And so here we have people doing the final networking.

3:24 - And also what they’re doing is, you’ll see they’re bringing out these power cables onto the top. In this system the power is actually delivered from the ceiling as well. So we won’t see in this video, but in the final system these cables are attached up the way into power which comes in above. That’s as far as this video goes in the construction of MARCONI. I hope you found that interesting and instructive.

### Text

Wee ARCHIE is very small and was built on someone’s desk. Real supercomputers are very large and require a lot of infrastructure to support them and manpower to build them.

This time lapse video documents the installation of the PRACE MARCONI system at CINECA in Bologna, Italy. We use it to pick out various aspects of supercomputer hardware that are not so well illustrated by Wee ARCHIE.

Is there anything that surprised you? We are curious to know so share your impressions in the comments section!

MARCONI @ CINECA - Power connections
MARCONI - power connections (ph. MMLibouri) (image)

MARCONI @ CINECA - Power and networking cables
MARCONI - cables (image)

## Processors, ARCHER and Wee ARCHIE (Quiz)