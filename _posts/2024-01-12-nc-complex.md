---
title: nc-complex
subtitle: Complex numbers in netCDF
author: Peter Hill
tags: netcdf file-format
software:
   licence: MIT
   author: Peter Hill
   github: https://github.com/PlasmaFAIR/nc-complex
layout: software
---

`nc-complex` is a lightweight, drop-in extension for netCDF that
handles reading and writing complex numbers. Currently there are C and
C++ APIs, and it has been integrated into [netcdf4-python][netcdf4]. A
Fortran API is also planned.

The `nc-complex` library understands most of the [major existing
conventions](https://nc-complex.readthedocs.io/en/latest/#conventions-used-in-applications)
for storing complex numbers, including as a compound datatype or as a
dimension of size two, and should work for any netCDF file format.

I wrote `nc-complex` as I was getting frustrated with the lack of
native support for complex numbers in netCDF files, leading to people
using a whole variety of different ways of storing them. The netCDF
developers were not very keen on expanding their specification to
include complex numbers, despite users requesting them for a decade or
more. This is also at least partly due to HDF5 _also_ not having
native support for complex numbers.

This project was partly about this new library that I wrote, but also
partly a campaign to try and get better support for complex numbers
into these popular file formats. To this end, I wrote a reasonably
comprehensive discussion of the issue in the `nc-complex` readme, and
provided input into discussions in the [CF-conventions][cfconventions]
and HDF5 communities. The outcome of this was getting a proposal
accepted for getting native complex number support into HDF5,
hopefully opening the possibility for eventual native support in
netCDF as well.

[netcdf4]: http://unidata.github.io/netcdf4-python/
[cfconventions]: https://github.com/orgs/cf-convention/discussions/370
