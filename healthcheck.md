---
title: Health-checks
subtitle: How we asses your software
layout: page
---

Our health-check is a lightweight review of your code and software project. This
will give you an idea of how sustainable your software is, and if there's
anything you can do to improve it. This _isn't_ an in-depth code review of every
line in the code-base -- though if this is something you would like, we're happy
to discuss!

This checklist is meant as a starting point for discussion, rather than an
comprehensive assessment of your project. We will go through each item with you,
and discuss how it applies to your specific project, what things are currently
sustainable, and places where we think improvements could be made, if any! We'll
then work with you to make some of those improvements to your project.

See the [application process](./index.html#how-it-works) on how to get started.

This checklist is based on the [review
checklist](https://joss.readthedocs.io/en/latest/review_checklist.html) from the
[Journal of Open Source Software](https://joss.theoj.org) (JOSS). The checklist
is under the [MIT
licence](https://github.com/openjournals/joss/blob/master/LICENSE).

### General checks

- **Accessibility:** Is the source code, or information on how to access the
  source code, available via a public URL?
- **Version control:** Is the source code under version control?
- **License:** Does the repository contain a plain-text LICENSE file with the
  contents of an [OSI approved](https://opensource.org/licenses/alphabetical)
  software license?

### Documentation

- **A statement of need:** Do the authors clearly state what problems the
  software is designed to solve and who the target audience is?
- **Installation instructions:** Are there instructions on how to compile or
  install the software?
- **Dependencies:** Is there a clearly-stated list of dependencies? Are
  known-good or tested versions listed? For example, minimum compiler version.
- **Example usage:** Are there examples of how to use the software (ideally to
  solve real-world problems)?
- **Functionality documentation:** Is the core functionality of the software
  documented to a satisfactory level (e.g., API method documentation)?
- **Tests:** Are there automatic or manual tests so that the functionality of
  the software can be verified? How coarse-grained are the tests, for example
  are there unit tests as well as system tests?
- **Community guidelines:** Are there clear guidelines for third parties wishing
  to 1) Contribute to the software 2) Report issues or problems with the
  software 3) Seek support
- **Continuous integration/development:** Are tests automated to run on changes
  to the version control repository? Is packaging/publishing of the software
  automated? Is there any automatic linting or static analysis performed?
- **Citations:** Is there a DOI for the software itself? Are relevant papers
  linked from the repository?
- **Releases:** Are there regular releases of the software, following [semantic
  versioning](https://semver.org)?

### Functionality

- **Installation:** Does installation proceed as outlined in the documentation?
- **Functionality:** Have the functional claims of the software been confirmed?
- **Performance:** If there are any performance claims of the software, is
  evidence provided?
