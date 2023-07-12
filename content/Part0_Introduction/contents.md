# Introduction to High Performance Computing

This course aims to introduce the principles of high performance computing (HPC) systems, how they operate, and how we can utilise the computing power they can offer to model complex systems. 

This will start with a discussion of supercomputers (HPC systems), their purpose, hardware, and trends in computing. We will then cover material on the fundamentals of modern computers and how we can perform computations in parallel. This will lead into a discussion of how supercomputers derive their performance, and how distributed memory architecture is used in HPC systems.

We will run through a concrete simulation example, discussing the details of the problem and how it can be modelled on a computer. We will then detail how parallel programming can be applied to problems to speed up a calculation, before discussing performance in parallel programs. Finally we will discuss how simulations are developed and used to make predictions, and some real world examples.


```{prereq}

To take part in the exercises you will need:
   - Access to the {{ machine_name }} system
   - Basic Knowledge of Git
   - Basic Knowledge of Bash

```

---

## Arrangements for this self-service course

This course has already run in other forms in the past and we are keen the material remains available to the community, therefore the course has been presented in this new format with support from the EuroCC project. This course is hosted on github  and will run in an unfacilitated form meaning the course will not have involvement, input and direction from the Educators. However it will be monitored, so please therefore raise issues on the [git repository](https://github.com/EPCCed/Intro-to-HPC-self-service/issues) which the course is hosted from, if there are comments on the material. 

---

## Welcome

```{raw} html
<iframe id="kaltura_player" width="700" height="400" src="https://cdnapisec.kaltura.com/p/2010292/sp/201029200/embedIframeJs/uiconf_id/32599141/partner_id/2010292?iframeembed=true&playerId=kaltura_player&entry_id=1_dkfu13gm&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;flashvars[Kaltura.addCrossoriginToIframe]=true&amp;&wid=1_ju8ohj3q" width="400" height="285" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" sandbox="allow-downloads allow-forms allow-same-origin allow-scripts allow-top-navigation allow-pointer-lock allow-popups allow-modals allow-orientation-lock allow-popups-to-escape-sandbox allow-presentation allow-top-navigation-by-user-activation" frameborder="0" title="Welcome_hd"></iframe>
```

```{solution} Transcript
0:12 - Welcome to the Supercomputing MOOC. My name is David Henty, and I work at EPCC, the Edinburgh Parallel Computing Centre at the University of Edinburgh in Scotland. We run the UK National Supercomputer Service, ARCHER, and it’s used by a wide range of scientists and engineers across the United Kingdom. ARCHER is also part of the European PRACE research infrastructure, which is a pan-Europeon network of supercomputers, and this whole course has been developed as part of PRACE.

0:43 - Today’s supercomputers are the largest and most powerful calculating machines ever constructed. Costing tens of millions of euros to build and consuming millions of euros of electricity per year. But how are they constructed? Where do they get their enormous computing power from? Are they similar to your home laptop or completely different? How can we use all that computational power to solve real problems? What real-world applications do supercomputers have? Over the next five weeks, we’re going to help you understand what supercomputing is. And although we’ll use some particular supercomputers as specific examples, we’ll try and cover most things at a conceptual level, so you understand the fundamental principles and challenges involved.

1:27 - In this course, we have four main aims, to give you some sense of the excitement of working at the leading edge of computing technology, to demystify the technical jargon and explain how supercomputers are built and how they are programmed, to show you how useful they are and the central role they play in modern science and engineering, and to explain how real-world problems such as weather forecasting can be expressed in a way that can then be tackled by a modern supercomputer. Now this isn’t a programming course. Although if you do know about computer programming, you’ll be able to appreciate the similarities and differences between programming your home machine and programming a parallel supercomputer.

2:07 - We also don’t assume any existing knowledge of computing maths or science. We just assume you’re generally interested in computing and how computers can be used to solve real-world problems. The course also aims to give you an insight into how your domestic devices, such as netbooks, laptops, and games machines, actually work. A modern mobile phone would have beaten the fastest supercomputer in the world less than 25 years ago. So where does all this power come from? What drives this seemingly relentless growth in computer technology? And what does the future hold?
```

This may be the first self-service course you have worked through, so to help you get the best learning experience we have prepared a few tips for you. Hopefully, they will help you get the most out of the course.

The course material is presented as a mixture of videos, articles, exercises and quizzes. The course is no longer fully facilitated so we would like to encourage you to interact with others taking the course if many of your local team are working through the content. If there is anything that you find confusing, ask, and if you know an answer to a question, answer it. You will find that both asking and answering questions helps to consolidate not only your but also other people’s knowledge.

---


```{figure} ./../Part1_Supercomputing/images/BayesInterior.jpg
```

## About EPCC

Since 1990, EPCC has developed a reputation for leading-edge capability in all aspects of High Performance Computing (HPC). Based at the University of Edinburgh, we work with industry, academia, public and third-sector organisations to promote the adoption and value of HPC.

With a team of more than 110 highly qualified specialists, we have an exceptional pool of talent. Our engineers and technical staff have a powerful combination of practical and theoretical knowledge that keeps EPCC at the forefront of industry and research.

```{raw} html
<iframe width="700" height="400" src="https://www.youtube.com/embed/NEgbVNIo560" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```

EPCC aims to accelerate the effective exploitation of novel computing throughout industry, academia and commerce. 

This has been our mission for over 30 years as we strive to be the UK’s premier Supercomputing centre, renowned internationally for innovation and leading-edge research.

Our work is guided by key goals and objectives that support this mission and place EPCC at the heart of the Exascale era.

Delivering world-class services
We will continue to deliver world-class High Performance Computing (HPC) and data services to benefit our users and partners. As well as supporting businesses with our advanced computing facilities and capabilities.

Leading data-driven innovation 
We are a pivotal element in the success of the Edinburgh and South East Scotland City Region Deal and its Data-Driven Innovation initiative.

Collaborating globally
We will continue to build and maintain collaborations and strong relationships with key organisations worldwide, undertaking globally-important computing research.

Promoting teaching excellence
We endeavour to maintain our position as a major provider of HPC training.

---

```{figure} ./../Part1_Supercomputing/images/large_hero_a8f32791-5354-4a66-aeb0-79352dedae18.jpg
```

This course has been adapted with EuroCC funding to update the original course developed under PRACE and improve course flexibility to allow it to be tailored to multiple machines' needs while keeping the core theme of the course.

## Acknowledgements

This material has been developed by James Richings, Stephen Farr, Manos Farsarakis, David Henty and Weronika Filinger at EPCC under funding from EuroCC.
This was developed from original material developed, with additional contributions from Zheng Meyer-Zhao at Surfsara, under funding from PRACE.

This new course benefits from the Sphinx template developed by the Swedish NCC which was also funded by EuroCC.
