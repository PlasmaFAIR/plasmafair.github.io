---
title: tree-sitter-fortran
subtitle: Fortran grammar for popular parsing library
author: Peter Hill
tags: Fortran parsing
software:
    licence: MIT
    author: Matt Stadelman
    github: https://github.com/stadelmanma/tree-sitter-fortran
layout: software
---

Tree-sitter is a popular library for parsing programming languages for
syntax highlighting and code navigation in IDEs, websites, and other
tools. Although modern Fortran is still widely used in academia, many
syntax highlighting packages, for example, have not kept up with the
developments in the language, and fail to highlight modern Fortran
correctly. A Fortran grammar was already being developed, and was
about 60% complete when we started to contribute.

This project was useful because we could build other tools on top of
it, as well as hopefully getting better syntax highlighting in our
favourite editors and websites! It was also a classic case of needing
to build the tools in order to build the tools -- in order to find
what needed adding or fixing in the grammar, I downloaded a large
corpus of Fortran codes (around 200 at last count, around 34M lines of
code!), and wrote a short python tool to parse Fortran files in a
directory with the tree-sitter grammar and display syntax errors with
some context. This made it very easy to quickly find missing features
and edge cases in the existing grammar.

While some Fortran features were very easy to add to the grammar, there are a
few things that are very tricky to parse. For example, the language
doesn't have reserved keywords, and so it's necessary to manually
allow clashes between the `identifier` AST node and many
keywords. Line continuations are also allowed in some strange places,
and we don't try to parse all of these -- in the middle of other
tokens, for instance! The other particularly tricky thing to support
is the preprocessor. While there is no official Fortran preprocessor,
most compilers use a fairly standard C preprocessor, which does
straight-forward textual replacement -- which means it can do
basically _anything_ to the source. This isn't possible to completely
support in the grammar, but I was able to get a minimal, common subset
in.

My work on the grammar took it from being able to parse about 60% of
files in the corpus to being able to parse 92%, with the majority of
the failures coming from preprocessor macros (several projects use
something like a `DEBUG_WRITE` macro which expands to `write` or
nothing, for instance) or are actual syntax errors. Excluding those
puts it much closer to 99% of all files.
