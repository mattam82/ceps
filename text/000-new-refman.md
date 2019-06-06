- Title: Reference Manual 2.0

- Drivers: Théo Zimmermann, Jim Fehrel, Matthieu Sozeau

----

# Summary

This CEP concerns an overhaul of the reference manual to make it:
- better structured
- more accessible and caring to different audiences
- more comprehensive and synchronized with the implementation of Coq

# Motivation

The current reference manual grown organically for many years, and it is
showing in its apparent "disintegration", whereby new chapters, even on
core features, are added as addendums. It also lacks a summary-birds eye
view of the different parts/features of the system and accessibility to
different audiences (new user, power user, developer, academic
interested in the theory, industrial user interested in the practice,
...). Finally, it lacks documentation of some care features like
unification or coinductive types, which only get explained through
examples.

# Detailed design

Split presentation in two views:
- Mathematical/Computer Science user view
- Theoretical/insider/developer's view

More examples, incremental presentation: first usage, with a large
audience, then exhaustive technical explanations adressed to the
expert/developer. Attribute features to contacts/maintainers.

- Acknowledge the different views, e.g. on tactics: we don't want to run
into debates of alternative solutions here.

- Open ended / usage view

- Missing a detailed view of the system:
* How is it organized?
* What is the TCB?
  High-level and precisely
* How to review a Coq library (coqchk, printing)

- Missing a documentation of the elaboration process.

- constr:(), "Vernacular": really from the user point of view: terms,
  commands.


- Coq standard library introduction:
* TODO Actually introduce definitions and their usage with examples.
  How does it relate to stdlib2?

Potential Technical Requirements:
* TODO Very long chapters, split them up, does sphinx allow that?
* TODO Relatedly, possibility to build intermediate tables of contents

Ideal overall structure:

* Core calculus. PCuIC
  -- No use of elaboration here! Actually should avoid using Gallina at
  all. A checker should be able to do that.
** Summary/highlights
** Terms
*** Correspondance to actual Coq syntax (for uses)
*** Correspondence to actual ML encoding (for devs)
** Typing
** Conversion/Cumulativity "expanded"
** Universes and Universe Polymorphism
** SProp
** CoInductives
** Native Integers
** VM and native compute
** Module system
** Libraries (dev view)
** Extraction


* Gallina and elaboration, related vernacular commands

** Summary/highlights
** Syntax
** Terms with holes
** Unification (*new*)

*** First needs a high-level view probably.

*** TODO Integrating the paper on unification:
    https://www.irif.fr/~sozeau/research/publications/drafts/unification-jfp.pdf
*** First-Order Unification
*** Pattern Unification
*** Heuristics of Coq
*** TODO Ask Beta for collaboration?

** Universes and Minimization
** Extended pattern-matching
** Notations
** Implicit Arguments
*** Bidir hints for example?
** Coercions
** Type Classes
** Canonical Structures
** Module System
** Library managment (user view: scoping of defs etc...)
** Other commands


* Proof Languages

** Summary/highlights
** Interactive model, proof handling
*** Parallel proof processing
** Tactics
** Ltac/Ltac2
*** Examples should be integrated there
** ssr
** Other proof languages? At least mention Mtac2!
** Decision procedures
*** Omega/Micromega
*** Ring
*** Nsatz
*** Generalized rewriting

* Developing

** Structuring projects
*** Library managment (user view: examples)
** Coq commands
** coqdoc
** coq_makefile, making plugins
** IDEs: coqide, proof-general, ...

2 tasks to start with, more to come: make issues for each task:
* Jim has a prototyp mlg -> sphinx converter (ensure sync)
** Starting with the Ltac chapter (maybe a bit more than fixiing grammar).
* Splitting up/restructuring the whole document.

Steps/roadmap:
- CEP on the structure but also the process:
* Make an example restructuring
* Once overall structure is agreed, work in two steps:
** First split as much as possible the chapters and insert placeholders
** Then consolidate and create the parts

# Drawbacks

<!-- Is the proposed change affecting any other component of the system? -->
<!-- How? -->

None!

# Alternatives

Yes, do the related works.  What do other systems do?

# Unresolved questions

Questions about the design that could not find an answer.


