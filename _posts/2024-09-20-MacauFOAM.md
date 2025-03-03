---
title: MacauFOAM
subtitle: OpenFOAM-based low-temperature plasma simulation
author: Peter Hill
tags: C++ simulation PIC
software:
    licence: Closed source
    author: Jan Trieschmann
layout: software
---

MacauFOAM is an extension for OpenFOAM for simulating low-temperature
plasmas using the particle-in-cell (PIC) method. The code has been
used in several publications, and unfortunately is currently
closed-source due to the institutional policies.

As usual, we started with our health check, reviewing the project's
codebase, documentation, tests, and so on. This turned out to be a bit
unusual -- while there were design choices for the project layout and
some code constructions that I would definitely argue against in most
C++ projects, actually these were following the OpenFOAM style very
closely, and so I ended up rating the project reasonably
healthy. OpenFOAM is a big project with many users, so restructuring
this code and rewriting in a more conventional modern C++ style would
both be a significant effort, and could end up being a net negative.

There were two small pieces of work we agreed with the project lead
though, the first one being to replace their pseudo-random number
generator (PRNG) with something more robust. The existing
implementation used the C++ standard library PRNG Mersenne-Twister,
which has many issues when used for Monte-Carlo simulations (such as
in PIC!). A very good modern PRNG is
[xoshiro](https://prng.di.unimi.it), and it was very simple to adapt
this to the C++ PRNG framework, and drop it in to MacauFOAM.

The second piece of work was to get CI up and running for them. The
software repository is on an institution-hosted GitLab instance, so
this was not quite as simple as setting it up for public GitHub repos,
as it involved getting a VM setup as a runner, and some manual
intervention. After the usual back-and-forth involved in setting up CI
on new projects, the code and now builds and runs the existing smoke
tests in a VM with OpenFOAM pre-installed.
