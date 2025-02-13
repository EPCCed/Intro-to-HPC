# Understanding Supercomputing

```{figure} ./images/large_hero_fa026d7f-4b55-44e4-a94f-051574e1bd87.jpg
© iStock.com/adventtr
```

## Understanding Supercomputing - Processors

So what are supercomputers made of? Are the building components really so different from personal computers? And what determines how fast a supercomputer is?

In this step, we start to outline the answers to these questions. We will go into a lot more detail next week, but for now we will cover enough of the basics for you to be able to understand the characteristics of the supercomputers in the [Top500](https://www.top500.org/lists/top500/2022/06/) list (the linked list is from June 2022).

It may surprise you to learn that supercomputers are built using the same basic elements that you normally find in your desktop, such as processors, memory and disk. The difference is largely a matter of scale. The reason is quite simple: the cost of developing new hardware is measured in billions of euros, and the market for consumer products is vastly larger than that for supercomputing, so the most advanced technology you can find is actually what you find in general-purpose computers.

When we talk about a processor, we mean the central processing unit (CPU) in a computer which can also be considered as the computer’s brain. The CPU carries out the instructions of a computer program. The terms CPU and processor are generally used interchangeably. The slightly confusing thing is that a modern CPU actually contains several independent brains; it is actually a collection of several separate processing units, so we really need another term to avoid confusion. We will call each independent processing unit a CPU-core - some people just use the term core.

A modern domestic device (e.g. a laptop, mobile phone or iPad) will usually have a few CPU-cores (perhaps two or four), while a supercomputer has tens or hundreds of thousands of CPU-cores. As mentioned before, a supercomputer gets its power from all these CPU-cores working together at the same time - working in parallel. Conversely, the mode of operation you are familiar with from everyday computing, in which a single CPU-core is doing a single computation, is called serial computing.

Interestingly, the same approach is used for computer graphics - the graphics processor (or GPU) in a home games console will have hundreds of cores. Special-purpose processors like GPUs are now being used to increase the power of supercomputers - in this context they are called accelerators. We will talk more about GPUs in Week 2.

```{figure} ./images/large_hero_8408f33c-87f5-4061-aec7-42ef976e83fd.webp

A typical CPU has a small number of powerful, general-purpose cores; a GPU has many more specialised cores. © NVIDIA

```

To use all of these CPU-cores together means they must be able to talk to each other. In a supercomputer, connecting very large numbers of CPU-cores together requires a communications network, which is called the interconnect in the jargon of the field. A large parallel supercomputer may also be called a Massively Parallel Processor or MPP.

Does it surprise you to learn that games console components and other general-purpose hardware are also used in supercomputers?

© SURFsara


```{figure} ./images/large_hero_9c54a5dc-d810-4c39-aa57-92c15bb303d2.jpg

© iStock.com/firina
```
## Understanding Supercomputing - Performance

The Top500 list ranks supercomputers by their floating-point performance - let’s take a look at what that means.

In supercomputing, we are normally interested in numerical computations: what is the answer to 0.234 + 3.456, or 1.4567 x 2.6734? Computers store numbers like these in floating-point format, so they are called floating-point numbers. A single instruction like addition or multiplication is called an operation, so we measure the speed of supercomputers in terms of floating-point operations per second or Flop/s, which is sometimes written and said more simply as Flops.

So how many Flops can a modern CPU-core perform? Let’s take a high-end processor like the AMD EPYC Zen2 (Rome) 7F32 CPU (which happens to be the processor used in the Snellius system at SURFsara). The way a processor is normally marketed is to quote its clock frequency, which here is 3.7 GHz. This is the rate at which each CPU-core operates. Clock speed is expressed in cycles per second (Hertz), and the prefix Giga means a billion (a thousand million), so this CPU-core is working at the almost mind-blowing rate of 3.7 billion cycles per second. Under favourable circumstances, an AMD EPYC CPU-core can perform 16 floating-point operations per cycle, which means each CPU-core can perform 16 x 3.7 billion = 59.2 billion floating-point operations per second.

So, the peak performance of one of our CPU-cores is 59.2 GFlops.

We say peak performance because this is the absolute maximum, never-to-be-exceeded figure which it is very hard to achieve in practice. However, it’s a very useful figure to know for reference.

Clearly, with many thousands of CPU-cores we’re going to encounter some big numbers so here is a table summarising the standard abbreviations you’ll come across:

|Ops per second  |	Scientific Notation	Metric | Prefix	| Unit |
|---|---|---|---|
| 1 000	| 10^3  |	Kilo	| Kflops |
| 1 000 000 |	10^6|	Mega	Mflops |
| 1 000 000 000	| 10^9 |	Giga |	Gflops |
| 1 000 000 000 000 |	10^12 |	Tera |	Tflops |
| 1 000 000 000 000 000 |	10^15 |	Peta |	Pflops |
| 1 000 000 000 000 000 000 |	10^18 |	Exa | Eflops |

```{note}

A quick word of warning here: when talking about performance measures such as Gflops, we are talking about powers of ten. For other aspects such as memory, it is more natural to work in powers of 2 - computers are binary machines after all.

```

It is something of a coincidence that 2^10 = 1024 is very close to 10^3 = 1000, so we are often sloppy in the terminology. However, we should really be clear if a kiloByte (Kbyte) is 1000 Bytes or 1024 Bytes. By KByte, people usually mean 1024 Bytes but, strictly speaking, a Kbyte is actually 1000 Bytes. The technically correct terminology for 1024 Bytes is KibiByte written as KiByte.

This might seem like an academic point since, for a KByte, the difference is only about 2%. However, the difference between a PByte and a PiByte is more than 12%. If your supercomputer salesman quotes you a price for a PetaByte of disk, make sure you know exactly how much storage you’re getting!

© SURFsara

```{figure} ./images/large_hero_b1c107b7-2f0d-4e8c-bdd8-10ac0bf6d4dd.jpg
© iStock.com/bowie15
```
## Understanding Supercomputing - Benchmarking

If we are going to compare performance we need some standard measure. When buying a car, we don’t just take the manufacturer’s word for how good the car is - we take it for a test drive car and see how well it performs in practice. For cars we might be interested in top speed or fuel economy, and it turns out that we are interested in the equivalent quantities for supercomputers: maximum floating-point performance and power consumption.

To measure supercomputer performance the test drive is how fast it can run a standard program, and this process is called benchmarking.

In the supercomputer world, two terminologies are often used to measure the performance of a system, Rpeak and Rmax. Rpeak is the theoretical peak performance, which is just the peak performance of a CPU-core multiplied by the number of CPU-cores. Rmax is the maximum performance achieved while running the LINPACK benchmark.

LINPACK involves running a standard mathematical computation called an LU factorisation on a very large square matrix of size Nmax by Nmax. The matrix is just a table of floating-point numbers, and the rules of the benchmark allow you to choose how big Nmax is.

As of June 2018, the world’s fastest supercomputer (the Summit system at the Oak Ridge National Laboratory in the USA) used Nmax of over 16 million - imagine working with a spreadsheet with over 16 million rows and 16 millions columns!

Supercomputers are not only expensive to purchase, but they are also expensive to run because the power consumption can be huge. A typical supercomputer consumes multiple megawatts, and this power is turned into heat which we have to get rid of.

For example, the fourth on the top500 list, Tianhe-2 system has a peak power consumption of 18.5 megawatts and, including external cooling, the system drew an aggregate of 24 megawatts when running the LINPACK benchmark. If a kilowatt of power costs 10 cents per hour, Tianhe-2’s peak power consumption will be 2400 euros per hour, which is in excess of 21 million euros per year.

Rpeak and Rmax are what Top500 uses to rank supercomputers. Also quoted is the electrical power consumption, which leads to the creation of another list - the [Green 500](https://www.top500.org/lists/green500/2022/06/) (June 2022)- which ranks supercomputers on their fuel economy in terms of Flops perWatt. Despite its massive power bill, Frontier is quite power-efficient. The top ranked system (Frontier) holds 2nd position on the Green 500.

Take a look at the [Top500](https://www.top500.org/lists/top500/2022/06/) list  - does the fact the top supercomputer for performance is also the to for power efficeny surprise you? What could be the reason for this?

© SURFsara

---

```{figure} ./images/large_hero_9150f201-3215-41f7-8ae2-d88821e7525a.jpg
© iStock.com/RichVintage
```
## Supercomputing Word Search

To help you familiarise with the supercomputing terminology we have prepared a word search for you to print out!

```{figure} ./images/large_hero_9748869f-e962-4c23-a6b6-8216e757920c.png
```

The word search contains 19 words that have been mentioned in the previous steps. Can you find all of them? Do you know how to explain all of them?

---

```{figure} ./images/large_hero_81d5664c-efa9-46cd-9129-547c2167f93a.jpg
© iStock.com/ThomasVogel
```
## HPC System Design

Now you understand the basic hardware of supercomputers, you might be wondering what a complete system looks like. Let’s have a look at the high-level architecture of supercomputer, mainly pointing out how it differs from a desktop machine.

The figure below shows the building blocks of a complete supercomputer system and how they are connected together. Most systems in the world will look like this at an abstract level, so understanding this simple figure will give you a good model for how all supercomputers are put together.

```{figure} ./images/large_hero_a3db6ae7-8a0e-4fe4-b2da-302380de963a.png
```

Let’s go through the figure step by step.

### Interactive Nodes

As a user of a supercomputer, you will get some login credentials, for example a username and password. Using these you can access one of the interactive nodes (sometimes called login nodes). You don’t have to travel to the supercomputer centre where these interactive nodes are located - you just connect from your desktop machine over the internet.

Since a supercomputer system typically has many hundreds of users, there are normally several interactive nodes which share the workloads, i.e. to make sure that all the users are not trying to access one single machine at the same time. This is where you do all your everyday tasks such as developing computer programs or visualising results.

### Batch System

Once logged into an interactive node, you can now run large computations on the supercomputer. It is very important to understand that you do not directly access the CPU-cores that do the hard work. Supercomputers operate in batch mode - you submit a job (including everything needed to run your simulation) to a queue and it is run some time in the future. This is done to ensure that the whole system is utilised as fully as possible.

The user creates a small file, referred to as a job script, which specifies all the parameters of the computation such as which program is to be run, the number of CPU-cores required, the expected duration of the job etc. This is then submitted to the batch system. Resources will be allocated when available and a user will be guaranteed exclusive access to all the CPU-cores they are assigned. This prevents other processes from interfering with a job and allows it to achieve the best performance.

It is the job of the batch scheduler to look at all the jobs in the queue and decide which jobs to run based on, for example, their expected execution time and how many CPU-cores they require. At any one time, a single supercomputer could be running several parallel jobs with hundreds waiting in the queue. Each job will be allocated a separate portion of the whole supercomputer. A good batch system will keep the supercomputer full with jobs all the time, but not leave individual jobs in the queue for too long.

### Compute nodes

The compute nodes are at the core of the system and the part that we’ve concentrated on for most of this week. They contain the resources to execute user jobs - the thousands of CPU-cores operating in parallel that give a supercomputer its power. They are connected by fast interconnect, so that the communication time between CPU-cores impacts program run times as little as possible.

### Storage
Although the compute nodes may have disks attached to them, they are only used for temporary storage while a job is running. There will be some large external storage, comprising thousands of disks, to store the input and output files for each computation. This is connected to the compute nodes using fast interconnect so that computations which have large amounts of data as input or output don’t spend too much time accessing their files. The main storage area will also be accessible from the interactive nodes, e.g. so you can visualise your results.

© SURFsara

---

```{figure} ./images/large_hero_0f828adf-3adc-4c16-b8b3-77614555e059.jpg
© iStock.com/marekuliasz
```

## What Supercomputing is not ...

One of the main aims of this course is to de-mystify the whole area of supercomputing.

Although supercomputers have computational power far outstripping your desktop PC, they are built from the same basic hardware. Although we use special software techniques to enable the many CPU-cores in a supercomputer to work together in parallel, each individual processor is basically operating in exactly the same way as the processor in your laptop.

However, you may have heard of ongoing developments that take more unconventional approaches:

- Quantum Computers are built from hardware that is radically different from the mainstream.
- Artificial Intelligence tackles problems in a completely different way from the computer software we run in traditional computational science.
We will touch on these alternative approaches in some of the final week’s articles. In the meantime, feel free to raise any questions you have about how they relate to supercomputing by commenting in any of the discussion steps.

---

## Supercomputing Terminology

```{questions} Question 1
If someone quotes the performance of a computer in terms of “Flops”, what do they mean?

A) the total number of floating-point operations needed for a computation

B) the number of float-point operations performed per second

C) the clock frequency of the CPU-cores

D) the total memory

```

```{solution}

B) - it’s our basic measure of supercomputer speed.

```

```{questions} Question 2
What is a benchmark in computing?

A) a hardware component of a computer system

B) a computer program that produces scientific results

C) a computer program specifically designed to assess performance

D) the peak performance of a computer

```

```{solution}

C) - it’s the equivalent of a standard consumer test for a product, like “how many litres per second can my electric shower deliver water at 40 degrees centigrade?”

```

```{questions} Question 3
Which one below represents the right order of number of Flops (from small to large)?

A) Kflops Gflops Mflops Tflops Eflops Pflops

B) Mflops Gflops Kflops Pflops Tflops

C) Kflops Mflops Gflops Tflops Pflops Eflops

D) Gflops Kflops Tflops Mflops Pflops Eflops

```

```{solution}

C) - computers started in the Kiloflops range and we are now approaching Exaflops.

```

```{questions} Question 4
What does clock speed typically refer to?

A) the speed at which a CPU-core executes instructions

B) memory access speed

C) the I/O speed of hard disks

D) the performance achieved using the LINPACK benchmark

```

```{solution}

A) - it’s basically the heartbeat of the processor.

```

```{questions} Question 5
What processor technologies are used to build supercomputers (compared to, for example, a desktop PC)?

A) a special CPU operating at a superfast clock speed

B) a large number of special CPUs operating at very fast speeds

C) a large number of standard CPUs with specially boosted clock speeds

D) a very large number of standard CPUs at standard clock speeds

```

```{solution}

D) - we take standard desktop technology but use vast numbers of CPUs to increase the computational power.

```

```{questions} Question 6
A parallel computer has more than one CPU-core. Which of the following are examples of parallel computers?

A) a modern laptop

B) a modern mobile phone

C) a supercomputer

D) a home games console

E) all of the above

```

```{solution}

E) - That’s right - parallelism is ubiquitous across almost all modern computer devices and is not restricted to just a few high-end supercomputers.

```