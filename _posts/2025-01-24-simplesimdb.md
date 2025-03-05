---
title: simplesimdb
subtitle: A lightweight simulation data creator and database manager
author: Liam Pattinson
tags: python utility data management
software:
    github: https://github.com/mwiesenberger/simplesimdb
    license: MIT
    author: Matthias Wiesenberger
layout: software
---

[Simplesimdb][simplesimdb] is a lightweight Python library for managing the creation and
management of simulation data. It is capable of tracking input parameters and linking
them to a local store of output files, and therefore avoid rerunning expensive
simulations for which the input parameters have been seen before. It also permits users
to search a set of previous input parameters to determine which outputs already exist.

Our health check of simplesimdb found that the code was written to a high standard, so
our work was instead focused on upgrading some of the surrounding infrastructure. This
included updating the packaging setup to use `pyproject.toml`, formatting with
[`black`][black], linting with [`ruff`][ruff], and setting up CI to automate testing,
linting and publishing. The in-code documentation was also upgraded to use [NumPy doc
style][npdoc], type hints were added throughout, and a set of online docs was created
using [Sphinx][sphinx] and [readthedocs][rtd].

We encountered some unexpected challenges, as the author preferred to keep the project
as simple as possible. This included a requirement that the whole library should consist
of a single Python module at the top-level of the project. This meant that some routine
upgrades did not work as expected, and alternative tools had to be found. For instance,
it was found that [setuptools][setuptools] could not install a single module as a Python
package, despite their documentation insisting otherwise! After some experimentation, it
was found that [hatchling][hatchling] provided a solution. In addition, we'd gotten into
the habit of reusing Sphinx configurations between projects, but in this case we had to
go back to basics and almost start from scratch. This turned out to be a very useful
exercise, as I picked up a few new tricks in the process!

[simplesimdb]: https://github.com/mwiesenberger/simplesimdb
[black]: https://black.readthedocs.io/en/stable/index.html
[ruff]: https://docs.astral.sh/ruff/
[npdoc]: https://numpydoc.readthedocs.io/en/latest/format.html#docstring-standard
[sphinx]: https://www.sphinx-doc.org/en/master/index.html
[rtd]: https://about.readthedocs.com/
[setuptools]: https://setuptools.pypa.io/en/latest/setuptools.html
[hatchling]: https://hatch.pypa.io/1.9/config/build/#build-system
