- Title: Typeclasses, proof-search

- Drivers: Matthieu Sozeau, Cyril Cohen, Théo Zimmermann

- Status: Draft

----

# Summary

- Generalize Equivalent keys
- Namespaces / levels of abstraction: control unfolding. CEP #5
- Configurability of the pattern-matching, unification and refinement
  tactic, transparency information (based on namespaces).
  
  Idea: Hint Resolve foo | priority pattern, using syntactic
  pattern-matching and full conversion...

- Trace: see Ltac 2.0
- Mémoisation: Ltac 2.0 too?

- Controlling shelving behaviors.

- Recognizing holes: 
  * typeclass to be solved
  * subgoal
  * implicit to be filled by unification
  
  Naming and refering to those.


# Current situation

The whole hint/automation system is based on a pre-typeclasses view of
automation, with a fixed set of strategies associated to Hint Resolve
hints and two different restrictions for applying hints, computing
priorities and performing proof search in auto/eauto (not even mentionning
all the variants dauto, prolog...). On the side of this, the
[typeclasses eauto] resolution engine has evolved to a full-fledged
proof search tactic handling dependent subgoals, shelving and
backtracking natively. These three different implementations cause
confusion for users, and make the code three times more complicated.



# Proposed solution

KISS: replace the current auto/eauto/typeclasses eauto tactics by a
single implementation. This involves making this procedure easily
customizable to maintain a good level of compatibility when ones to use
[auto]'s restriction to hints that do not produce dependent subgoals for
example, or call "unification" instead of "evarconv".  Using our modern
tactic engine it should be possible to express these restrictions at a
high level of abstraction, the specificities of each of the old tactics
should hence be apparent in the code.

In this view, the old [auto with db] would become an alias for ``search
using auto_strategy with db`` where ``auto_strategy`` defines how ``Hint
Resolve`` hints are interpreted by ``auto`` (e.g. call unification, do
not allow dependent subgoals).

Naming hints.

## Overview

```ocaml
```

## Pros

## Cons


## Potential solutions to the above issues

### Code duplication
