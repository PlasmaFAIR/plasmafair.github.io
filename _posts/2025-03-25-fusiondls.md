---
title: FusionDLS
subtitle: A Python package for simulating detachment in next-generation tokamak divertors
author: Liam Pattinson
tags: python tokamak detachment divertor eqdsk geqdsk edge SoL
software:
    github: https://github.com/FusionDLS/FusionDLS
    license: LGPL3.0
    author: Cyd Cowley, Mike Kryjak
layout: software
---

[FusionDLS][fusiondls] is a Python project that solves for the position of
plasma detachment fronts in tokamak divertors. By solving a reduced model,
FusionDLS is capable of quickly producing parameter scans over various divertor
designs and magnetic geometries to optimise the design of divertors for
next-generation devices.

When we began work on FusionDLS, the project was comprised of a collection of
Python modules and Jupyter notebooks in the top level of its GitHub repository.
As our first step, we reorgansed the project into an installable package, as
this would serve as a basis for all of our future upgrades. We then set up
formatting and linting with [ruff][ruff], converted some of the existing
Jupyter notebooks into integrated tests, and created some HTML docs for the
project using [Sphinx][sphinx]. All of this was automated using GitHub actions.

The Jupyter notebooks were reworked to serve as examples for users of the
project, and we were able to integrate them into the online docs using the
[myst-nb][mystnb] Sphinx plugin. To ensure the notebooks were kept up-to-date
with the rest of the project, we used the [nbmake][nbmake] pytest plugin
to run the notebooks in our testing CI jobs.

We also make some substantial upgrades and feature additions to FusionDLS.
Previously, much of the data within FusionDLS was passed around using plain
dictionaries, and by converting these to [dataclasses][dataclasses] we were
able to provide stronger guarantees about data types and availability at each
stage of the simulation. The conversion to an object-oriented design also
allowed us to define a number of new utility methods and properties to clean up
repetitive code.

Finally, we added the ability to trace scrape-off layer field lines from
G-EQDSK files -- a commonly used format for tokamak equilibria. This was made
much easier using a combination of [FreeQDSK](/2023/04/06/FreeQDSK) to read the
files and [FreeGS][freegs] to trace the field lines.

[fusiondls]: https://github.com/FusionDLS/FusionDLS
[ruff]: https://docs.astral.sh/ruff/
[sphinx]: https://www.sphinx-doc.org/en/master/index.html
[nbmake]: https://pypi.org/project/nbmake/
[dataclasses]: https://docs.python.org/3/library/dataclasses.html
[freegs]: https://github.com/freegs-plasma/freegs
