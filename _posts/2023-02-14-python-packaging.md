---
title: Python Packaging
subtitle: A lesson on Python packaging and publishing, built using the Carpentries Workbench 
author: Liam Pattinson
tags: teaching
software:
    url: https://carpentries-incubator.github.io/python_packaging/
    github: https://github.com/carpentries-incubator/python_packaging
    licence: CC-BY 4.0
    author: Liam Pattinson
layout: software
---

Due to its ease of use, expressiveness, and thriving open-source ecosystem, Python has
become one of the most popular programming languages in modern research. It is commonly
used as a scripting language, meaning researchers write their code as a series of
expressions in a single file, which is then executed by the Python runtime. However,
there are many limitations to this scripting model, and it can be more sustainable in
the long run to structure Python code into _libraries_ instead of scripts. The benefits
of doing so include:

- Code can be compartmentalised across multiple files and directories, improving the
  modularity of a project and better encapsulating the internal data and routines.
- Projects become installable using tools such as `pip`, meaning they can be run from
  anywhere on the user's system.
- Dependencies can be stated explicitly, and will be automatically downloaded and
  installed along with the target package.
- Projects can be uploaded to [PyPI][PyPI], allowing others to easily install them from
  the command line.
- Many powerful tools such as linters, unit testing frameworks, and documentation
  generators can be more easily integrated with your code.
- Python libraries can retain script-like behaviour with the inclusion of a
  `__main__.py` file.

However, the process of creating a Python library (or 'package') has always been
somewhat obscure, and is rarely taught in an academic setting. To aid researchers in
understanding this topic, I wrote a lesson using the new [Carpentries
Workbench][Carpentries Workbench]: a framework written in R for creating accessible
teaching materials. Lesson designers can build lessons by writing them in R
markdown (an extension to regular markdown, itself a simple markup language), and the
tools in the Workbench make it easy to build and deploy them in the form of easily
navigable web pages.

Our lesson on Python packaging is fairly comprehensive, and covers the following
material:

- Converting a simple script into a reusable module, composed of several importable
  functions.
- Migrating from a single module to a package containing multiple modules.
- Writing a `pyproject.toml` file to describe your project and make it installable.
- Structuring packages in such a way as to hide the private implementation details and
  make the public API more accessible.
- Publishing packages on PyPI using the tools `build` and `twine`.
- Managing the ongoing development of a package with regular releases and meaningful
  version numbers.
- Retaining script-like behaviour, and how to write better scripts using
  [`argparse`][argparse].

The core materials teach best practices using the newest community standards, as
expressed in PEP [517][PEP-517], [518][PEP-518], and [621][PEP-621]. The lesson also
contains an optional module that provides a brief overview of the tools and techniques
that were previously considered 'standard' packaging methods, such as the `distutils`
library and the use of `setup.py`, as these methods still feature prominently in many
online tutorials and popular open-source packages.

The lesson has been accepted in the Carpentries incubator, although further work will be
required to restructure the lesson into a deliverable software workshop.

[PyPI]: https://pypi.org/
[Carpentries Workbench]: https://carpentries.github.io/workbench/
[argparse]: https://docs.python.org/3/library/argparse.html
[PEP-517]:https://peps.python.org/pep-0517/
[PEP-518]:https://peps.python.org/pep-0518/
[PEP-621]:https://peps.python.org/pep-0621/
