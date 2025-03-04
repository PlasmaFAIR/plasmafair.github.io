---
title: epydeck, epyscan, sdf-xarray
subtitle: EPOCH Python tools
author: Peter Hill
tags: EPOCH python PIC
software:
   licence: BSD-3
   author: Peter Hill, Joel Adams
   github: https://github.com/PlasmaFAIR/sdf-xarray
layout: software
---

This project was a set of brand new tools for working with EPOCH
simulations: `epydeck` for reading/writing input files, `sdf-xarray` for
reading output files into
[xarray](https://docs.xarray.dev/en/stable/), and `epyscan` for
parameter scans and active learning. We wrote these tools as separate
packages in order to facilitate generic workflows, as EPOCH users have
fairly diverse needs and this helps avoid tight dependencies and
locking users into a rigid workflow.

[`epydeck`](https://github.com/PlasmaFAIR/epydeck) is a very minimal
EPOCH input file ("deck") reader/writer. The file format is pretty
simple, with only a few quirks, so `epydeck` was mostly
straightforward, and works with plain Python `dict`s. Input decks
consist of a series of named blocks with key-value pairs, somewhat
similar to the ubiquitous INI files. The more interesting features
are: repeated blocks, repeated keys, and missing values. The latter
was very easy to handle, just convert to/from a bool. Repeated keys
also turned out to be simple to deal with, the values are put into a
list on deserialisation, and lists are serialised to repeated
key-value pairs. Repeated blocks were slightly trickier, and ended up
being converted to/from nested `dict`s using the `name` key from the
block.

For simplicity, `epydeck` doesn't do any verification of the blocks or
keys, and doesn't do any parsing of expressions or units. As we're
planning on handing off these projects to the main EPOCH developers,
they may try to add these capabilities using the
[EPOCpp](2023-12-19-epocpp) implementation.

[`sdf-xarray`](https://github.com/PlasmaFAIR/sdf-xarray) is another
minimal package that adds an xarray backend for the
[SDF](https://github.com/Warwick-Plasma/SDF) file format. At first, I
attempted to use the existing SDF-Python interface, but that turned
out not to expose all the functionality of the underlying SDF C
library that is required to make a well-behaved xarray backend, so I
ended up writing a new SDF Python interface using Cython. This was
actually very straightforward, and much easier to maintain than the
existing Python module, written in plain C.

Lastly, [`epyscan`](https://github.com/PlasmaFAIR/epyscan) is yet
another minimal package, this time designed to aid "campaigns" of
simulations. The main feature is to take a template EPOCH input file,
and a `dict` of a new sample -- a set of new values for some inputs --
and create a new directory in a campaign structure, with a new input
file corresponding to that sample. It comes with a couple of basic
samplers for uniform grid scans and quasi-Monte Carlo samples over
some parameter space, but it can also be very easily used in an active
learning loop, or optimisation workflow.
