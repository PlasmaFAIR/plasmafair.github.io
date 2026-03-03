---
title: Fortitude
subtitle: Lightning fast Fortran linting
author: Liam Pattinson
tags: Fortran linter linting developer tools tooling rust ruff tree-sitter 
software:
   licence: MIT
   author: Liam Pattinson, Peter Hill
   github: https://github.com/PlasmaFAIR/Fortitude
layout: software
---

Fortran developer tooling has long lagged behind the options available to other
languages. A Python developer might have previously chosen between
[Pylint][pylint] and [Flake8][flake8] to lint their code, or may have used a
combination of both. They may have extended them with a number of third-party
plugins, and nowadays will likely have replaced both with [Ruff][ruff]. All of
these tools are fully capable in their own right, each filling a slightly
different niche. The landscape is very different in Fortran, with a scattering
of tools such as [Flint][flint] and [Camfort][camfort], but no clear frontrunner.
Open-source Fortran linters typically have some combination of:

- Lacking features
- Difficult installation procedure
- Lack of customisation
- Parsing difficulties
- Often raise false positives
- Poor performance
- Not actively maintained
- Idiosyncratic/controversial style opinions

[Fortitude][fortitude] was developed as an in-house PlasmaFAIR tool in order to
patch this hole in the Fortran ecosystem. I started work on a prototype in
January 2024 while working on [SUMMIT](/2024/11/29/summit.html), as during
refactoring I encountered a lot of repetitive code smells and opportunities for
modernisation, but found `grep` and `sed` to be much too unreliable to hunt
down every last style violation. These included issues such as:

- Missing `implicit none`
- Use of `double precision` over `real(dp)`.
- Lack of floating point suffixes, which can lead to unexpected precision loss
  (e.g. `sqrt(2.0)` vs `sqrt(2.0_dp)`.

Writing a linting tool from scratch is a huge undertaking, but for my prototype
I only needed to find a handful of code patterns. In addition, the hardest part
of the problem -- parsing Fortran -- could be solved by relying on an existing
Fortran parser. Knowing that Peter had been working on
[tree-sitter-fortran](/2025/03/04/tree-sitter-fortran.html),
[tree-sitter][treesitter] was the obvious choice. The basic design of the
software was also simplified by taking inspiration from an existing tool. Being
a big fan of Ruff, I took some time reading through their codebase and coming
up with a rough outline of how everything should fit together. Copying Ruff's
homework also had one wonderful side-effect: it gave me the opportunity to
learn [Rust][rust], which has quickly become my favourite programming language!

The first working prototype was ready in just a few days, and I immediately put
it to use on SUMMIT. I became distracted by other projects afterwards, and it
wasn't until [RSECon24][rsecon24] that I decided to revisit Fortitude, as while
attending the Back to the Fortran Future workshop (credit to Andrew Brown and
Austen Rainer at Queens University Belfast!) it became clear to me that the
lack of quality developer tooling was causing real problems for Fortran
developers, especially younger RSEs who were more accustomed to working in
Python or C++.

Discussions on this topic continued throughout RSECon24, where I learned that
another RSE had also developed a tree-sitter based linter specifically for the
CASTEP project, [castep-linter][casteplinter]. After comparing their design
with that of Fortitude, I was able to make a few performance enhancements, and
was amazed to find Fortitude was running up to hundreds of times faster than
some of its open-source competitors.

Peter started working on Fortitude shortly after, and the project truly took
off. Rather than using Ruff simply as inspiration, we started borrowing
language-agnostic pieces of Ruff directly (MIT License be praised!), and our
feature set grew dramatically. Fortitude was noticed by some users of [Fortran
Discourse][discourse], and the response was broadly positive. Over the next
year, we continued picking up new users and contributors, all the while
expanding Fortitude's rule set, customisability, and features. As a side-effect
of Fortitude's growing popularity, tree-sitter-fortran was truly put to the
test on the wide variety of real-world Fortran projects, and the resulting
improvements to the grammar will be of benefit to any other tools that use it.

A notable moment of community engagement came when I organised a [Fortran
Tooling Hack Week](/2025/06/09/fortran-tooling-hack-week.html), and a number of
attendees chose to contribute to Fortitude. The results were several new rules
and some progress towards a format mode that would work similarly to
[fprettify][fprettify]. We also presented a workshop at the sequel to Back to
the Fortran Future at [RSECon25][rsecon25], where attendees were asked to
install Fortitude, explore it's features, and provide feedback. A number of
people were already familiar with the tool, and the responses we received
helped to focus our attention on what users felt were lacking. Shortly
afterwards, I presented Fortitude to [FortranCon 2025][fortrancon25], where we
were again warmly received and it was clear that we already had a few fans in
the audience!

One of the most exciting recent additions is an implementation of the Language
Server Protocol (LSP), which allows Fortitude to integrate into text editors
and provide real-time feedback. It can be tested most easily using our
[extension for Visual Studio Code][vscodeextension].

We still have a few major features on the roadmap, including the aforementioned
formatting mode, improved support for the C-preprocessor, and a refactor to
permit deeper semantic analysis of the code. Preprocessor support has been
requested by many users due to its prevalence in real-world code, but as it
operates by simple text replacement, it isn't possible to rationally parse
Fortran code containing preprocessor directives and macros without performing
the preprocessing step in full. For that reason, we've started work to
implement our own C-preprocessor, but it will be a challenge to make it feature
complete and capable of tracking locations in the preprocessed code back to the
original.

Deeper semantic analysis will allow us to add much more sophisticated linting
rules. Currently, Fortitude iterates directly over the Concrete Syntax Tree
(CST) generated by tree-sitter, but this means many objects lack any qualifying
information. For example `foo(5)` might be a function or an array, and right
now it isn't possible for us to tell the difference. Full semantic analysis
would mean performing a first pass over the code before linting in which we
compile information about variables and types. With this completed, we will be
able to plug many holes in our rule set, including the notably absent
`unused-variable` and `undeclared-variable`. Given the large number of rules
already implemented (over 80!), a significant refactor would be needed to
implement this.

There's also good news in future for the Fortitude community, as I was recently
awarded an [SSI Fellowship][ssi] to promote the formation of a community of
practice around Fortitude. With this fellowship, I intend to travel to numerous
UK institutes to provide master classes on Fortitude and assist with the
implementation of features that matter the most to them. Long term, the aim is
to pivot from our current development model to a community-driven open-source
model.


[pylint]: https://www.pylint.org/
[flake8]: https://flake8.pycqa.org/en/latest/
[ruff]: https://docs.astral.sh/ruff/
[flint]: https://gitlab.com/cerfacs/flint
[camfort]: https://camfort.github.io/
[fortitude]: https://fortitude.readthedocs.io/en/stable/
[treesitter]: https://tree-sitter.github.io/tree-sitter/
[rust]: https://rust-lang.org/
[rsecon24]: https://rsecon24.society-rse.org/
[casteplinter]: https://github.com/byornski/castep-linter
[discourse]: https://fortran-lang.discourse.group/
[fprettify]: https://github.com/fortran-lang/fprettify
[rsecon25]: https://rsecon25.society-rse.org/
[fortrancon25]: https://events.fortrancon.org/event/1/
[vscodeextension]: https://marketplace.visualstudio.com/items?itemName=PlasmaFAIR.fortitude
[ssi]: https://www.software.ac.uk/programmes/fellowship-programme
