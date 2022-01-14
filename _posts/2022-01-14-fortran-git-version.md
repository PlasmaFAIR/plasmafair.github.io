---
title: fortran-git-version
subtitle: Automatic version numbering in Fortran from git tags
author: Peter Hill
tags: Fortran git version
software:
    url: https://github.com/PlasmaFAIR/fortran-git-version
    github: https://github.com/PlasmaFAIR/fortran-git-version
    licence: BSD-3-Clause
    author: Peter Hill
layout: software
---

`Fortran-git-version` extracts Fortran package version information from a `git`
repository, instead of hard-coding it in a source file.

`Fortran-git-version` is a very short library that I originally wrote for
[GS2][gs2], and have now pulled out into a standalone library for use in other
projects. This library does essentially one thing: compile the output of `git
describe` into a Fortran library in such a way that it won't cause unnecessary
rebuilds of your entire project.

Here's the problem that `fortran-git-version` is trying to solve: we'd prefer
not to hard code the version number in our source files, and have to remember to
update it when we release a new version. Instead, if we're creating `git` tags
on releases, is there someway we can capture that information? Can we go further
and capture the exact commit when we build?

There are two problems we need to overcome:

1. How do we automatically capture the information from `git` when we build, and
   ensure that this information is always up-to-date?
2. We'll need to re-compile at least one file when we create a new commit, for
   instance, so that the built artefact contains the commit hash. How can we
   avoid a recompilation cascade of all the other Fortran modules that might
   transitively depend on this file?

The exact details of the solution to the first problem differ somewhat between
what build system we're using, but the idea is the same (at least for CMake and
Makefiles). We write the output of the `git` commands we're interested in to
some temporary file, and we can then check if this file changes between
builds. If it does, we trigger a recompilation.

The second problem is actually much simpler: we make use of a Fortran
submodule. If you're familiar with the split between C/C++ headers and
implementation files, then you'll see the similarities. With a Fortran
submodule, we have an `interface` block in the main `module`, containing the
procedure interfaces, while a second file has a `submodule` with the actual
implementation of these procedures. The result is that we only need to recompile
modules that `use` our module if the interfaces change. We use this to pass the
information from `git` via preprocessor macros and recompile just the submodule
file when the info changes (we still need to link the built objects together,
but that is unavoidable).

The result is a library with a handful of functions that return the version
number (from `git describe`), the commit hash (from `git rev-parse HEAD`), the
date of the latest commit (from `git show`), and whether or not the working tree
is clean (from `git diff-index`). These functions will always return the most
up-to-date information when you build your project, without triggering
unnecessary recompilations.


[gs2]: https://gyrokinetics.gitlab.io/gs2/
[setuptools_scm]: https://github.com/pypa/setuptools_scm
