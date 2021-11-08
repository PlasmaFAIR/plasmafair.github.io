---
title: inference-tools
subtitle: Bayesian data analysis
author: Peter Hill
tags: python Bayesian data-analysis
software:
    url: https://github.com/C-bowman/inference-tools
    github: https://github.com/C-bowman/inference-tools
    licence: MIT
    author: Chris Bowman
layout: software
---

Inference-tools is a Python package providing a collection of tools for Bayesian
data analysis. It provides implementations of Markov chain Monte Carlo (MCMC)
algorithms, density estimating and plotting, and Gaussian-process regression and
optimisation.

My first impression of the project was that it was already doing a lot of things
right: it has a good README, describing what it does and how to install it; it
has a good set of documentation, including examples and extensive API
documentation; it was already packaged and installable via `pip` and
PyPI.

Talking to the author, Chris, we worked out a few things that I could to help
improve the project, mainly to add some automation and to tighten up the tests,
as well as to re-combine the standalone documentation repo with the main
codebase.

I now have a nice set of GitHub Action workflows for Python that I can easily
drop-in to new projects to automate [testing][test-action],
[packaging][python-publish], and [formatting][black-action]. The testing action
only runs on changes to `.py` files, which reduces unnecessary CI jobs from
changes to the documentation, for example. It also builds the package and runs
`twine check` to test the packaging before a release.

The automated packaging uses the official [PyPI publish Github
Action][pypi-publish] which makes uploading a new version trivial. I also used
[setuptools_scm][setuptools_scm] to set the version from the latest git
tag. This now means that creating a new release on Github automatically bumps
the version number at the same time as triggering the action which builds and
uploads the package to PyPI. This automatic version number is also now used in
the Sphinx `conf.py`, and we've now removed the possibility of forgetting to
update a hardcoded version number!

As I mentioned, inference-tools already had a pretty extensive test suite. To
find out how I could improve it, I first used [pytest-cov][pytest-cov] to
measure the coverage -- which was 60%! This is pretty good going for a
scientific package. Digging into the tests further, I realised that there were
some tests which were missing `assert`s (these tests were very likely
re-purposed from examples, which is actually a great way of writing tests as
they can then serve double-duty as documentation on how to use the code, as well
as tests).

To write more tests I used a combination of tools to help me explore the
untested space: coverage and mutation testing. I've been interested in mutation
testing for awhile, but had not got round to using it, so this seemed like a
great opportunity to try it out. The premise of mutation testing is to use a
tool to make random changes, "mutants", to your source code and then run your
tests -- if they still pass then the mutant is said to have survived, if they
fail the mutant was killed. Survivors are an indication that your tests are not
comprehensive -- it's possible to change the source and still pass the tests!

I used [mutmut][mutmut] for this. Mutmut makes a change to the code and runs
your tests for you, and can generate HTML pages showing you the diffs that
survived. It has a cache of previously run mutants and can use coverage
information, which is a really nice feature, meaning the run can be stopped and
restarted as you like. My process was to run mutmut on a single file, and use
the surviving mutants to guide me in where and how to write new tests.

There are a couple of downsides to mutation testing, the main one being that it
is very expensive: the test suite has to be run for each mutant, and each line
or expression may generate multiple mutants. If your tests take more than a few
seconds to run, and your codebase is more than a couple of thousand lines, it
can a _long_ time to run across the whole code. The other downside is that
mutants are generally limited to changes to single tokens or expressions, for
example turning `x * y` into `x / y`, `array[0]` into `array[1]`, or `True` into
`False`. The current generation of tools are not able to generate larger, more
structural changes to the code, which would perhaps help capture more realistic
bugs.

I found some limited use for two other testing tools: [hypothesis][hypothesis]
and [freezegun][freezegun]. I'll talk more about hypothesis in future
posts. Freezegun is very useful for any tests that involve time somehow. For
inference-tools, it was for a method `run_for(days, hours, minutes)` that runs
an MCMC chain for a set period of time. Obviously running a test for multiple
days of real time is a non-starter, but with freezegun I was able to have
complete control over time and verify the implementation of `run_for`!

Overall I was very impressed with the quality of inference-tools. Normally as
soon as I start writing tests for a piece of software I uncover bugs, but after
writing or expanding over 100 tests, I only found one bug and even that was
quite subtle and unlikely to be hit in real use.


[black-action]: https://github.com/C-bowman/inference-tools/blob/master/.github/workflows/black.yml
[freezegun]: https://github.com/spulec/freezegun
[hypothesis]: https://hypothesis.readthedocs.io/en/latest/
[mutmut]: https://mutmut.readthedocs.io/en/latest/index.html
[pypi-publish]: https://github.com/pypa/gh-action-pypi-publish
[pytest-cov]: https://pytest-cov.readthedocs.io/en/latest/
[python-publish]: https://github.com/C-bowman/inference-tools/blob/master/.github/workflows/python_publish.yml
[setuptools_scm]: https://github.com/pypa/setuptools_scm
[test-action]: https://github.com/C-bowman/inference-tools/blob/master/.github/workflows/tests.yml
