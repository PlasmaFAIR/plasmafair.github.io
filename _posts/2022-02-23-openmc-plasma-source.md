---
title: openmc-plasma-source
subtitle: OpenMC neutron sources for fusion applications
author: Liam Pattinson
tags: openmc plasma source 
software:
    url: https://github.com/fusion-energy/openmc-plasma-source
    github: https://github.com/fusion-energy/openmc-plasma-source
    licence: MIT
    author: Rémi Delaporte-Mathurin, Jonathan Shimwell et al.
layout: software
---

`openmc-plasma-source` is a Python package that simplifies the creation of
neutron sources for [OpenMC][openmc] Monte-Carlo simulations in Tokamaks. It
allows users to create point sources, ring sources, and randomised collections
of ring sources called `TokamakSource`'s.

The library is quite small, and as a result it was possible to make small
improvements over the whole project. First, the Python packaging system
was updated to use the PEP 517/518 method using a `pyproject.toml` file 
and `setup.cfg` file. Following this, the unit tests were greatly expanded.
To ensure all aspects of the library would work for a range of Tokamak geometries,
the [Hypothesis][hypothesis] library was used to test over hundreds of randomly
generated Tokamaks.

One of the most significant upgrades to the library was to introduce input validations
for all classes. These are used to ensure things such as negative radii aren't allowed,
or that Tokamak modes are restricted to 'H', 'L' or 'A'. Python [properties][properties]
are a great way to enforce these conditions, as by giving each attribute a custom
setter function, the user can avoid listing endless try/except blocks in the `__init__` 
function, and it protects against attributes being set to inappropriate values after
initialisation.

However, the class `TokamakSource` takes a lot of inputs, and this necessitates a lot of
properties. Defining two functions for each attribute is labour intensive and error
prone, and it can make navigating a Python file confusing. To automate the process, I
wrote a small library called [proper_tea][proper_tea] which provides 'property
factories' with descriptive names such as `positive_float()`, `int_greater_than(value)`,
or `in_set(iterable)`. Properties can then be simply declared as follows:

```python
class MyClass:

    pos_int = proper_tea.positive_int()
    abc = proper_tea.in_set(["A","B","C"])
    
    def __init__(self, pos_int, abc):
        self.pos_int = pos_int
        self.abc = abc
```

Now, If the user tries to set `pos_int` to `-5`, or `abc` is set to `"hello world"`, an exception is raised. 
After developing this, I was made aware of an existing package that does the same thing!
The [param][param] library is much more developed than `proper_tea`, and makes use of
[descriptors][descriptors] and a frightening amount of metaclassing rather than
properties. It was decided to use this instead of `proper_tea`, as it's much better
supported and is more likely to remain stable over time.

`openmc-plasma-source` is now a much better tested library, and a significant amount of
code was cleaned up during the upgrades. Using the [param][param] library, its classes
are also automatically protected against bad inputs, and as the class attributes are now
property-like, they are also protected against being set to an inappropriate value
elsewhere in the code.

[hypothesis]: https://hypothesis.readthedocs.io/en/latest/
[openmc]: https://github.com/openmc-dev/openmc
[properties]: https://docs.python.org/3/library/functions.html#property
[proper_tea]: https://github.com/LiamPattinson/proper_tea
[param]: https://param.holoviz.org/
[descriptors]: https://docs.python.org/3/howto/descriptor.html
