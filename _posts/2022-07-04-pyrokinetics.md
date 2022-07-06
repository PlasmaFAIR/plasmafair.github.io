---
title: Pyrokinetics
subtitle: A Python library to run and analyse gyrokinetics simulations.
author: Liam Pattinson
tags: gyrokinetics python
software:
    url: https://github.com/pyro-kinetics/pyrokinetics
    github: https://github.com/pyro-kinetics/pyrokinetics
    licence: LGPL3.0
    author: Bhavin Patel
layout: software
---

[Gyrokinetics][gyrokinetics] is a branch of theoretical plasma physics that aims to
provide a detailed description of plasma behaviour at length scales down to the
gyroradius. It is commonly used by tokamak scientists to describe the turbulent
transport effects that tend to dominate at the temperatures and pressures necessary to
achieve practical fusion energy.

The gyrokinetic equation is a gyro-averaged version of the Fokker-Planck equation, which
describes the evolution of a plasma distribution function in phase space.
Sub-gyroradius phenomena do not have much influence on macro-scale plasmas, so by
averaging over the gyroradius we may reduce the dimensionality of the phase space
distribution function from 7D (3 spatial, 3 velocity, 1 time) to 6D (3 spatial, 2
velocity, 1 time). Solving the resuling 6D equation is far from trivial, so for all but 
the simplest cases we must turn to computational methods.

A number of software packages have been developed for this purpose, such as [GS2][GS2]
and [CGYRO][CGYRO], and while each of these use different approaches to solve the
gyrokinetics equation, there is a large overlap in their respective domains. It is
common for gyrokinetics researchers to make use of multiple codes to ensure their
results are valid and replicable, but this requires learning the idiosyncracies of each
code and duplicating input files and analysis scripts to match each case. This can be
error prone, as each code makes use of different normalisations and defines geometrical
parameters in subtly different ways. [Pyrokinetics][pyrokinetics] is a Python library
that aims to standarise the researcher's interface with these codes, and it provides a
means to convert input files from one code to another. It also allows the user to gather
gyrokinetics output data to a standardised format, and it allows 'local' geometries and
species profiles (that is, those on a single tokamak flux surface) to be generated from
global equilibrium and kinetics data.

Our work on Pyrokinetics was divided into two sub-projects: firstly to implement a
plug-in system for each code type, and secondly to refactor a large portion of the
codebase. The plug-in system was implemented using a [factory pattern][factory], which
may be conveniently implemented in Python using the built-in [`UserDict`][collections]
abstract base class to achieve a `dict`-like interface. New classes may be 'registered' 
to the factory by assigning the class to a given key (achieved by providing a custom 
`__setitem__` method to the factory), and new instances of that class are constructed by 
indexing the factory with that key (uses the factory's custom `__getitem__` method):

```python
# Register to factory
my_factory["my_class"] = MyClass
# Build a new instance of MyClass [implied call to MyClass.__init__(self)]
my_class = my_factory["my_class"]
```

By subclassing `UserDict` and implementing custom `__setitem__` and `__getitem__`
methods, the factory automatically includes most `dict`-like features, such as the
`items()` or `get()` methods. The factory class allows Pyrokinetics developers to avoid 
the following code pattern:

```python
if code_type == "CodeTypeA":
    my_class = CodeTypeAClass()
elif code_type == "CodeTypeB":
    my_class = CodeTypeBClass()
elif ...
```

This pattern can be error prone, especially if it is repeated many times throughout a
large project, as the developers must remember to update every instance of the pattern
whenever a new subclass is added or an old one is updated. Using a factory, this pattern 
reduces to:

```python
my_class = my_factory[code_type]
```

This solution is much more robust to future code changes. The implementation in
Pyrokinetics also features automatic file type inference, which is achieved by passing a
file name in place of a code type:

```python
import pyrokinetics as pk
# Get a GKInputGS2 class from the gk_inputs factory    
gs2_input = pk.gk_code.gk_inputs["GS2"]
# Get the same by passing a GS2 input file
gs2_input = pk.gk.code.gk_inputs["gs2_input_file.in"]
# Get a GKInputCGYRO class by instead passing a CGYRO input file
cgyro_input = pk.gk_Code.gk_inputs["input.cgyro"]
```

This is achieved by implementing a short `verify()` function in each 'reader' class,
which attempts to read a file and checks that it contains some expected information. If
it does not, the `verify()` function raises an exception. The factory determines the
correct class type by iterating over each 'reader' class and calling their `verify()`
functions until one returns without having raised an exception. This adds a significant
overhead when reading files, as Pyrokinetics must iterate through each reader class,
attempt to read and verify the file for each of them, and finally read and process the
file in full after having determined the correct reader class. To circumvent this, the
user may optionally pass the code type alongside the file name to most file reading
functions in Pyrokinetics.

It is hoped that the new plug-in architecture will aid in the expansion of Pyrokinetics
to include more code types. It makes it easier for users to implement their own
Pyrokinetics plug-ins, as the requirements for a new plug-in are simply to implement
a new class and to register it with the relevant factory. For instance, the
external developer of "MyGyroCode" might implement a module for reading "MyGyroCode"
input files, and integrate it with Pyrokinetics as so:

```python
#===========================
# MyGyroCodePlugin.py
from pyrokinetics.gk_code import GKInput, gk_inputs

# Define gyrokinetics input file reading/writing class
class GKInputMyGyroCode(GKInput):
    # class definition goes here...

# Register with Pyrokinetics factory
gk_inputs["MyGyroCode"] = GKInputMyGyroCode

#===========================
# Some other script...
import pyrokinetics as pk
import MyGyroCodePlugin # this import will load the plug-in

pyro = pk.Pyro(gk_file = "my_gyro_code.in") 
```

The second stage to this project was a refactor to decouple the classes `Pyro` and
`GKCode`. The `Pyro` class is the central concept of Pyrokinetics, and it facilitates
the reading and writing of various files and allows the user to combine data in various
ways. `GKCode` was the base class for a collection of objects for reading/writing
gyrokinetics input/output files. Previously, these classes were closely linked, as
`Pyro` required a `GKCode` in order to process gyrokinetics files, but `GKCode` required
a `Pyro` object in order to perform most operations:

```python
pyro = pk.Pyro(gk_file="gs2_input.in", gk_code="GS2")

# internally, Pyro calls...

# Uses properties, sets self.gk_code to an empty GKCodeGS2
self.gk_code = gk_code 
# Pass self to GKCodeGS2.read. Modifies self.
self.gk_code.read(self, gk_file)
```

The `GKCodeGS2` instance created by the `Pyro` object would set the attributes of the
`Pyro` object rather than storing its own data. This implementation broke
[encapsulation][encapsulation], requiring developers to have a detailed understanding of
how both `Pyro` and `GKCode` objects worked in order to develop either one of them.
Furthermore, any changes to the internal workings of one of these classes would often
require corresponding updates to the others. The `GKCode` classes were similarly
responsible for setting up `GKOutput` instances, adding a further layer of coupling.

The refactor of this section of code included the following steps:

* Reimplement `GKCode` classes as `GKInput` classes, which own and manage gyrokinetics
  input data independent of `Pyro` objects.
* Implement `GKOutputReader` classes which process gyrokinetics output data independent
  of `GKInput` or `Pyro` objects. These objects were not designed to be stateful, but
  instead were tasked with receiving an input file and returning an [xarray][xarray]
  `Dataset` of gyrokinetics data. 
* Refactor `Pyro` to make use of these new classes instead of `GKCode`.
* At each step, implement unit tests and write documentation.

Following this refactor, some further work was carried out to allow `Pyro` objects to
maintain separate 'contexts' for each gyrokinetics code. There are cases where it may be
useful to convert a `Pyro` object from one gyrokinetics code to another, and then to
switch back to the original. Switching back and forth between gyrokinetics codes should
not destroy any information, so the capabilities of `Pyro` were expanded to permit
multiple independent contexts per `Pyro` object, and the rules of when data will be
overwritten or converted were formalised. 

There are already ongoing discussions about future updates to the Pyrokinetics API, so
it is likely that further refactoring will be required (in particular, the
context-switching behaviour of `Pyro` may need a rethink). However, by decoupling the
classes that make up the Pyrokinetics project, any further API changes will be much
easier to implement.

[GS2]: https://gyrokinetics.gitlab.io/gs2/
[CGYRO]: https://gafusion.github.io/doc/cgyro.html
[gyrokinetics]: https://en.wikipedia.org/wiki/Gyrokinetics
[pyrokinetics]: https://github.com/pyro-kinetics/pyrokinetics
[collections]: https://docs.python.org/3/library/collections.html
[factory]: https://sourcemaking.com/design_patterns/factory_method
[encapsulation]: https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)
[xarray]: https://docs.xarray.dev/en/stable/
