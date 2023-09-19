---
title: Epoch Containers
subtitle: Containerising an HPC particle-in-cell code
author: Liam Pattinson
tags: Epoch Docker Singularity Apptainer HPC PIC ICF
software:
   github: https://github.com/PlasmaFAIR/epoch_containers
   licence: GPLv3
   author: Liam Pattinson
layout: software
---

[Epoch][epoch] is a particle-in-cell (PIC) code widely used within plasma physics,
particularly in the regime of laser-plasma interactions. PIC codes aim to
self-consistently solve Maxwell's equations in the presence of a large number of charged
particles, many of which travel at relativistic velocities. Epoch and similar codes are
often used to provide insight into the physics of matter interacting with extremely
intense radiation, such as the conditions observed in inertial-confinement fusion
(ICF) experiments or many astrophysical phenomena.

Epoch is written in Fortran and is designed to be run on HPC systems. As is typical for
such software, there is a steep learning curve for new users, particularly those who
aren't experienced working from the Linux command line. During previous iterations of a
summer school masterclass at the York Plasma Institute, the set-up process for Epoch was
found to be a significant barrier-to-entry for some students, which limited the amount
of work they could complete during their short research projects. The aim of this
project was to see if _containerising_ Epoch could help to get new users running
simulations faster.

Containers are similar to virtual machines, in that they enable the packaging and
distribution of a complete computational workflow all the way down to the operating
system level. The key difference is that virtual machines pass all OS-level commands
through a hardware emulation layer, while containers are able to interface directly with
a target machine's hardware while restricting all activity to a protected memory space.

An oft-cited benefit to containers is that researchers can distribute complete workflows
with all dependencies already included, which makes it much easier for others to
replicate and build upon their results. The ability to share pre-built software and a
full stack of supporting libraries all the way down to operating system also makes it
easier to share complex software.

However, containerising HPC codes isn't a particularly straightforward process.
[Docker][docker] is the dominant containerisation platform by a huge margin, but the
Docker engine -- which is responsible for managing container images and memory volumes
on a user's system -- runs container processes with root privileges. As HPC system
administrators take a particularly dim view of their users having unrestricted root
access, Docker is unusable on HPC systems. For this reason, HPC codes must be
containerised using a tool such as [Singularity][singularity], which is specifically
designed to be compatible with HPC systems and tools such as MPI.

We chose a two-stage approach to containerise Epoch. First, a Docker container was built
containing multiple pre-compiled Epoch executables: one for each dimensionality (1D, 2D,
3D) using default compiler flags, and one more for each dimensionality with quantum
electrodynamic effects switched on. The advantage of creating a Docker container first
is that it can be easily tested locally, and there are many free tools available for
automatically building and publishing Docker containers via the GitHub Container
Repository (GHCR). This Docker container image was then used as a base to create a
Singularity image, which was hosted using the [Sylabs Container Services][sylabs].

The user interface to both the Docker and Singularity containers is managed using Python
scripts. These scripts simplify the process of interacting with the containers by
selecting the correct executable within the container, mounting the appropriate volumes,
and passing on input files.

One round of summer school students have now had experience running Epoch via
containers, and we received no complaints about their usability. However, there are
still some kinks to work out with the Epoch Singularity containers. The process to build
and publish Singularity images required a lot of manual input, as they were first built
using a Singularity recipe file via the Sylabs online build tool, then manually tested
on the University of York Viking cluster, digitally signed using the Singularity command
line tool, and finally published back to Sylabs. Ideally, this should be achievable
entirely via GitHub actions in an automated manner. One potential avenue for achieving
this may be to eliminate the Singularity build process entirely, and to modify the
Docker container such that it is directly runnable using either Docker or Singularity.

We also encountered some issues when linking the correct MPI libraries on systems
different to that on which the Singularity image was built. The solution to this problem
appears to be specific to each HPC machine, so we may have to revisit the example job
scripts provided to future students.

[epoch]: https://epochpic.github.io/
[docker]: https://www.docker.com/
[singularity]: https://sylabs.io/singularity/
[sylabs]: https://cloud.sylabs.io/
