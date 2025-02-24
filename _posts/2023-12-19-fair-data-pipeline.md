---
title: FAIR Data Pipeline
subtitle: Tracking the provenence of FAIR data
author: Liam Pattinson
tags: FAIR data pipeline C C++ Fortran CMake
software:
   github: https://github.com/FAIRDataPipeline
   licence: LGPLv3
layout: software
---

The [FAIR Data Pipeline (FDP)][fdp] is a collection of software utilities for
tracking the provenance of [FAIR][fair] (Findable, Accessible, Interoperable,
Reusable) data. It was originally developed to aid in the creation of
reproducible epidemiological modelling workflows, a need spurred by the outbreak
of COVID-19. FDP allows users to automate a data processing pipeline that traces
all inputs, outputs and associated metadata in a FAIR manner. APIs are available
in Python, R, Julia, C++, and Java.

Following a talk at [RSECon 2022][rsecon], we became interested in the potential
application of FDP to computational plasma physics pipelines, and offered to
expand its capabilities to include support for Fortran -- a lanugage commonly
used in plasma physics simulations and other high-performance computing
applications.  Rather than reimplementing the whole API in our target language,
as was the strategy employed for the existing supported languages, we instead
opted to reuse the [C++ API][fdpcpp] and instead create Fortran bindings.

The first step was to upgrade the CMake build system used by the C++ API so that
it would export targets to external projects. This proved to be more challenging
than expected, as multiple dependencies of FDP lacked adequate CMake support or
exported targets in a way that defied best practices.

Following this, a simple C API was implemented that deferred all computation to
the C++ objects via opaque pointer types. The C API was developed to act as a
bridge for the Fortran API, and also to serve as a jumping-off point for anybody
wanting to expand FDP's language support even further -- perhaps to something
like Rust or Haskell.

Finally, Fortran bindings to the C API were created using `bind(c)` interface
routines and `iso_c_binding` kinds. A first draft of the [Fortran API][fdpf] was
developed at a satellite hackathon for NERC's [Digital Gathering 2023][dg23] in
Cambridge, and further refinements were made afterwards. One of the biggest
challenges when writing this was to ensure compatibility between strings in
Fortran and C, as Fortran strings have a known size while C strings consist
simply of a pointer to the first character and a terminating null character
`'\0'` at some unknown offset in memory. While converting between the two
forms is trivial, it was found to be difficult to ensure correct lifetimes and
avoid unnecessary copies. Some unexpected behaviours of `bind(c)` routines were
also encountered, such as the requirement to attach the `value` attribute to
some dummy arguments in order to avoid passing junk data into the C functions.

[fdp]: https://www.fairdatapipeline.org/
[fair]: https://www.go-fair.org/fair-principles/
[fdpcpp]: https://github.com/FAIRDataPipeline/cppDataPipeline
[fdpf]: https://github.com/FAIRDataPipeline/FortranDataPipeline
[rsecon]: https://rsecon2022.society-rse.org/
[dg23]: https://digitalenvironment.org/events/dg23-digital-gathering-2023/
