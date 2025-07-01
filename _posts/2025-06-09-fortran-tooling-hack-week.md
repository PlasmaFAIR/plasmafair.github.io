---
title: Fortran Tooling Hack Week
subtitle: Improving developer tools for the Fortran community
author: Liam Pattinson
tags: Fortran tools tooling hackathon
software:
    github: https://github.com/PlasmaFAIR/FortranToolingHackWeek2025
    author: Liam Pattinson & Peter Hill
layout: software
---

[RSECon24][rsecon] featured a satellite event called [Back to the Fortran
Future][bttff] which brought together a number of Fortran developers to discuss
the challenges facing the community and plan some actionable solutions. The
same few problems were raised repeatedly:

- A lack of widely-agreed-upon community standards for Fortran best practices.
- Relatively few quality training courses.
- Immature or missing developer tooling compared to languages such as Python.

The last of those points was a definite pain point for me when I first started
writing Fortran. After some research, I was able to set up most of a modern
tool stack -- a [CMake][cmake] build system, [fprettify][fprettify] for
formatting, and [pFUnit][pfunit] for testing -- but with the exception of CMake,
the tools I found were hard to set up and lacking in features compared to
their equivalents in languages like C++ or Python.

After the satellite meeting, I contacted the organisers to see if I could
organise a follow-up hacakthon event to work on tooling. I already had some
experience in this domain from my work on [Fortitude][fortitude] (something
I'll definitely write up for this blog eventually!), and it was clear to me
that the community was eager to see some improvements.

The organisers of Back to the Fortran Future very helpfully provided me with
with some event planning advice and put me in contact with other interested
people in the community. I sent around a short survey to collect community
feedback on what sort of event they would like to see, and received a very
positive response. Following the results of the survey, I started planning to
run an in-person event, but unfortunately wasn't able to secure the necessary
funding, so I instead opted to run an online-only event. In addition, rather
than running a traditional 1-2 day hackthon, I chose to run a longer-duration
but lower-commitment 'hack week' in which the attendees were free to dedicate
as much or as little time as they wanted to their hack week projects and could
fit their contributions around their regularly scheduled work activities.

The event took place between Tuesday 27th and Friday 30th May 2025, and featured
contributions from 9 attendees:

- Made advances towards implementing a formatting mode for Fortitude. Some
  issues in its [Topiary][topiary] grammar were fixed, and steps were taken to
  reuse code style linting rules for more advanced formatting operations.
- Added four new linting rules to Fortitude, and progress was made towards a
  further two.
- Corrected a number of longstanding errors in
  [tree-sitter-fortran](/2025/03/04/tree-sitter-fortran).
- Investigated the possibility of adding [FPM][fpm] support to pFUnit.
- Worked towards a user-sortable reference of which features are offered by
  various compilers and tools.
- Resolved licensing issues that were preventing the inclusion of the
  [FORD][ford] documentation generator in Fortran-Lang community projects.

Though the attendance of the event was lower than I was hoping for, those who
stuck through to the end showed great commitment, and we made some good
progress. It was great to see people getting to grips with how developer tools
work, and some even learned new programming languages. Running the event was
more of a challenge than I'd expected, and I was effectively on-call for the
whole duration. In addition, while the 'hack week' format made the event more
accessible for some, the feedback collected afterwards suggested that a shorter
but more focused conventional format might have been more productive, as it was
difficult for some to schedule and prioritise their hackathon contributions
around their regular work.

Overall, this was an interesting event to run, and I'm looking forward to
applying what I learned to future events.

[rsecon]: https://rsecon24.society-rse.org/
[bttff]: https://lu.ma/ao471jms
[cmake]: https://cmake.org/
[fprettify]: https://github.com/fortran-lang/fprettify
[pfunit]: https://github.com/Goddard-Fortran-Ecosystem/pFUnit
[fortitude]: https://github.com/PlasmaFAIR/fortitude
[topiary]: https://github.com/tweag/topiary
[fpm]: https://fpm.fortran-lang.org/
[ford]: https://github.com/Fortran-FOSS-Programmers/ford
