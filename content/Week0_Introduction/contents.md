# Introduction to High Performance Computing

This course aims to inform users of the principles of how high perofrmance computer (HPC) systems operate and how to efficently utilise the computing power they can offer. The will start with a disucssion of HPC systems hardware and the common strucutures of HPC codes…

```{prereq}

   prerequisites

```

---

## Arrangements for this unfacilitated run

```{danger}

How do we want to present the structure of the document/course?

Week by week keeps this too close to the original Future learn course

```

This course has already run in other forms in the past and we are keen the material mantains avalible to the community in future. This course will run in an unfacilitated form meaning the course will not have substantial involvement, input and direction from the Educators. However it will be monitored and please therefore raise issues on the [github repository](https://github.com) which the course is hosted from if there are comments on the material. 

---

## Welcome

[comment]: <> (```{raw} html)
[comment]: <> (<iframe width="560" height="315" src="https://www.youtube.com/embed/Cj8n4MfhjUc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>)
[comment]: <> (```)

```{raw} html
    <video width="700px" height="500px" 
        controls="controls"/> 
        <source src="/home/jpr/Projects/EuroCC/MOOC/content/Week1_Supercomputing/video/Welcome_to_week1_hd.mp4" 
            type="video/mp4"> 
    </video>
```

```{raw} html
<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/2010292/sp/201029200/embedIframeJs/uiconf_id/32599141/partner_id/2010292?iframeembed=true&playerId=kaltura_player&entry_id=1_j7mdt8di&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;flashvars[Kaltura.addCrossoriginToIframe]=true&amp;&wid=1_tsiu9191" width="400" height="285" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" sandbox="allow-downloads allow-forms allow-same-origin allow-scripts allow-top-navigation allow-pointer-lock allow-popups allow-modals allow-orientation-lock allow-popups-to-escape-sandbox allow-presentation allow-top-navigation-by-user-activation" frameborder="0" title="Welcome_to_week1_hd"></iframe>
```

Welcome_hd


```{solution} Transcript
0:12 - Welcome to the Supercomputing MOOC. My name is David Henty, and I work at EPCC, the Edinburgh Parallel Computing Centre at the University of Edinburgh in Scotland. We run the UK National Supercomputer Service, ARCHER, and it’s used by a wide range of scientists and engineers across the United Kingdom. ARCHER is also part of the European PRACE research infrastructure, which is a pan-Europeon network of supercomputers, and this whole course has been developed as part of PRACE.

0:43 - Today’s supercomputers are the largest and most powerful calculating machines ever constructed. Costing tens of millions of euros to build and consuming millions of euros of electricity per year. But how are they constructed? Where do they get their enormous computing power from? Are they similar to your home laptop or completely different? How can we use all that computational power to solve real problems? What real-world applications do supercomputers have? Over the next five weeks, we’re going to help you understand what supercomputing is. And although we’ll use some particular supercomputers as specific examples, we’ll try and cover most things at a conceptual level, so you understand the fundamental principles and challenges involved.

1:27 - In this course, we have four main aims, to give you some sense of the excitement of working at the leading edge of computing technology, to demystify the technical jargon and explain how supercomputers are built and how they are programmed, to show you how useful they are and the central role they play in modern science and engineering, and to explain how real-world problems such as weather forecasting can be expressed in a way that can then be tackled by a modern supercomputer. Now this isn’t a programming course. Although if you do know about computer programming, you’ll be able to appreciate the similarities and differences between programming your home machine and programming a parallel supercomputer.

2:07 - We also don’t assume any existing knowledge of computing maths or science. We just assume you’re generally interested in computing and how computers can be used to solve real-world problems. The course also aims to give you an insight into how your domestic devices, such as netbooks, laptops, and games machines, actually work. A modern mobile phone would have beaten the fastest supercomputer in the world less than 25 years ago. So where does all this power come from? What drives this seemingly relentless growth in computer technology? And what does the future hold?
```


Welcome! Meet the lead educator of the Supercomputing course by PRACE, Dr David Henty who will guide you through the course.

This may be the first self service course you have worked through so to help you get the best learning experience we have prepared a few tips for you. Hopefully, they will help you get the most out of the course.

The course material is presented as a mixture of videos, articles, exercises and quizzes. Once you have gone through a step and decided that you have familiarised yourself with its content, you can mark it as complete.

The course is no longer fully facilitated so we would like to encourage you to interact with others taking the course if many of you local team are working through the content. If there is anything that you find confusing, ask, and if you know an answer to a question, answer it. You will find that both asking and answering questions helps to consolidate not only your but also other people’s knowledge. And more importantly, it’s fun! So if you have an opinion share it!

© EPCC at The University of Edinburgh

---

## About PRACE

```{danger}

How much do we want to focus on PRACE work and do we need to move this to a EuroCC focus

```

### Video

PRACE_hd


```{solution} Transcript
0:11 - High performance computing is essential for European scientific, industrial, and social advances, whether it’s preserving our environment, improving our health care, designing new technology, or transforming our economy. High performance computing is at the heart of innovation. The Partnership for Advanced Computing in Europe, PRACE, supports this objective, providing access to top level supercomputers. High performance computing combines all the biggest instruments used in science. It’s a telescope that enables us to view the entire universe, a microscope that lets us explore the quantum world at very small scales, a reactor that enables us to study plasmas and stars, and even a time machine that allows us to recreate the past or to predict the future.

1:02 - It helps modern medicine to develop drugs for widespread diseases more rapidly and produce individually tailored medicine more precisely. Energy efficient solutions can be modelled and analysed with supercomputers to develop alternatives and create a low carbon society. Climate change can also be tackled using simulations to improve weather forecasts and mitigate risks to food production and urban environments. For industry and SMEs, the use of high performance computing is becoming crucial in handling all big data issues and for planning the entire production chain. Numerical simulations can replace expensive and time consuming real life tests and prototypes. For example, PRACE supported a European car manufacturer to perform the first ever and to date biggest crash optimisation study.

1:53 - And through the SHAPE programme, it supports European SMEs to bring their innovative products to market faster. This strengthens the European economy, creating and securing jobs. Moving forward the next step in the evolution of high performance computing is the Exascale supercomputer, which will allow even faster simulations to bolster Europe’s place in the top tier of supercomputers.
```


This video, produced by PRACE, explains the aims of this international association.

PRACE stands for Partnership for Advanced Computing in Europe. The mission of PRACE is to enable high impact scientific discovery and engineering research and development across all disciplines to enhance European competitiveness for the benefit of society.

PRACE is established as an international not-for-profit association (aisbl) with its seat in Brussels. It has 24 member countries whose representative organisations create a pan-European supercomputing infrastructure, providing access to computing and data management resources and services for large-scale scientific and engineering applications at the highest performance level.

Both [EPCC](https://www.epcc.ed.ac.uk/) (at the University of Edinburgh) and [SURFsara](https://www.surf.nl/en/about-surf/subsidiaries/surfsara/) (in Amsterdam), the developers of this course, are active members of PRACE. EPCC hosts a PRACE Advanced Training Centre, and SURFsara was recently selected as one of four new PRACE Training Centres.

The national supercomputers at these centres, ARCHER and Cartesius, are Tier-1 systems within PRACE, accessible via [four different access types](https://prace-ri.eu/hpc-access/). Tiers are used to describe the hierarchy of supercomputers and supercomputing clusters on an international, national and local scale: Tier-0 are pan-European systems; Tier-1 are national systems; Tier-2 are regional systems; and Tier-3 are local clusters (mostly university resources).

The [Tier-0 computer systems](https://prace-ri.eu/hpc-access/hpc-systems/) and their operations accessible through PRACE are provided by five PRACE members: BSC representing Spain, CINECA representing Italy, ETH Zurich and CSCS representing Switzerland, GCS and HLRZ representing Germany and GENCI representing France. Four hosting members (France, Germany, Italy, and Spain) secured funding for the initial period from 2010 to 2015. In 2016 a fifth Hosting Member (CSCS, Switzerland) opened its system via the PRACE Peer Review Process to researchers from academia and industry.

For more information on all of the activities of PRACE including gaining access to its HPC facilities and training programmes, see the [PRACE website](https://prace-ri.eu/).

© PRACE

---

## About EuroCC

The [EuroCC](https://www.eurocc-access.eu/about-us/the-projects/) project aims to...

```{danger}
need to add some words about the EuroCC project
```


This course has been adapted with EuroCC funding to update the original course developed under Prace and improve course flexibility to allow it to be tailored to multiple NCC's needs while keeping the core theme of the course.

---
```{figure} ./../Week1_Supercomputing/images/large_hero_a8f32791-5354-4a66-aeb0-79352dedae18.jpg
cite!
```
## People

The lead educator, Dr David Henty, is responsible for HPC Training at EPCC, The University of Edinburgh.

The other educators are:

Weronika Filinger is an HPC applications consultant at EPCC, The University of Edinburgh.

James Richings is a HPC applications consultant at EPCC, The University of Edinburgh.

```{danger}
Who should be named publically associated with this material needs to be confirmed!

Additionally we don't have any interactivity so this needs removing
```

© EPCC at The University of Edinburgh