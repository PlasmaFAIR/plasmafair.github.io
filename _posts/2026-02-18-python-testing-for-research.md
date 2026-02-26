---
title: Python Testing for Research
subtitle: A training course on the practical use of tests in research software. 
author: Liam Pattinson
tags: Python testing pytest training Carpentries Workbench
software:
    github: https://github.com/ResearchCodingClub/python-testing-for-research
    author: Liam Pattinson & Ava Dean
layout: software
---

The University of York [Research Coding Club][rcclub] has been delivering
informal software training courses to staff and students over a number of
years, and during a hiatus over the 2024-25 academic year we had the
opportunity to rethink how we would organise and deliver our content. At the
[N8CIR][n8cir] Annual RSE Meetup in July 2025, we became aware of the
University of Sheffield's [FAIR² for Research Software][fair4rs] (FAIR²4RS)
project, an open-source educational resource for teaching intermediate software
skills at universities. With their help, we've been adapting their resources to
design our own new-and-improved [Research Coding Course][rccourse].

Several of the lessons within the FAIR²4RS course are built upon a customised
variant of the [Carpentries Workbench][workbench], using which one can develop
online software training materials from [R Markdown][rmarkdown]. I had some
experience of this from my previous work on a lesson on [Python
packaging](/2023/02/14/python-packaging.html), so it was fairly easy to get
started adjusting the course material to our preferences. A tricky part of the
process was to edit the University of Sheffield's variant of [Varnish][varnish]
-- the component of the Carpentries Workbench that sets the website styling and
templates -- to include our own branding. I wasn't able to build it locally, so
this had to be achieved by trial-and-error and pushing to GitHub!

With our [own version of Varnish][uoyvarnish] working, we set to work making a
few edits to the course material. It was of a high quality, so very little
needed changing. Due to the limited time we would have to teach it, we chose to
omit some material and substitute some topics we felt were missing; for
example, we replaced a section on testing data structures such as lists and
dicts with a section on testing floating point data with appropriate
tolerances. We also edited the section on regression testing to make use of our
own snapshot library, [snaptol][snaptol], and overhauled the CI lesson to
explore some of the benefits of CI, such as the ability to test on different
platforms than your own.

We unfortunately didn't have time to deliver all of the course material during
our session, but the experience will be informative when we're designing future
courses, which should now be much easier now we have our own variant on the
Carpentries Workbench up and running.

[rcclub]: https://researchcodingclub.github.io/
[n8cir]: https://n8cir.org.uk/
[fair4rs]: https://rse.shef.ac.uk/training/fair4rs/
[rccourse]: https://researchcodingclub.github.io/course/
[workbench]: https://carpentries.github.io/workbench/
[rmarkdown]: https://rmarkdown.rstudio.com/
[varnish]: https://carpentries.github.io/varnish/
[uoyvarnish]: https://github.com/ResearchCodingClub/uoy-varnish
[snaptol]: https://snaptol.readthedocs.io/en/stable/
