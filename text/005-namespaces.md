- Title: Namespaces

- Drivers: Jason Gross, Matthieu Sozeau, Cyril Cohen

- Status: Draft

----

# Disclaimer

Any syntax presented here is for presentation purposes only, please
concentrate on the concepts first!

# Summary

This document started by describes a proposal to satisfy
[feature request #4455](https://coq.inria.fr/bugs/show_bug.cgi?id=4455) and
[feature request #3171](https://coq.inria.fr/bugs/show_bug.cgi?id=3171) and solve
[JasonGross/coq-tools#16](https://github.com/JasonGross/coq-tools/issues/16)
by giving Coq a better notion of namespaces.

The idea was that users should be allowed to declare that certain constants should have user-chosen
kernel names.

We intend to generalize the notion of namespaces to cover
sections/modules and general handling of extra-logical data, allowing
namespaces to be extended, restricted, included and used (independently
from their extension). All this should be orthogonal from the actual
kernel global names, allowing to e.g. redefine a lemma by the same name
shadowing an older less general version. Moreover, namespaces can carry
extralogical information, namely hints, notations, opacity information,
implicit arguments and automatic foldind information, associated to a
constant or not. Naming notations, hints and configurations of any of
the above should be possible. 

Abstracting by a context (named as well) should be possible too, this is
linked to CEP #? by E. Tassi on optional arguments. Link to
Sections/Functors.

Namespaces should support set-algebra operations (unions, intersections
 for all named objects etc...). E.g removing hints, unioning
 transparency information (choosing a bias...) etc...



On the implementation side, we want to get rid of modules entirely and
rely instead on anonymous record (and in general inductive types).

Definition f := foo.
Put f in typeclass_instances.

Definition g := bla.
Put f in foo.bar.

Definition foo.g := bla. puts it in namespace foo only, not the current one.

# Syntax

The following are typical examples:
```coq
Namespace Coq.Init.
  Module Datatypes2.
    ...
  End Datatypes2.
End Coq.Init.
```

```coq
Local Namespace Init.
  Module Datatypes.
    ...
  End Datatypes.
End Init.
```

```coq
poly.v

Definition f := 0

polyext.v

Namespace poly.
  Use poly.
  Definition g := 1.
End poly
```

This would extend the _namespace_ `poly` with `f` and `g`, so that 

```coq 
Require poly polyext.
```

Gives use a namespace `poly` in which `f` and `g` live.
```coq
About poly.f 
```

Informs us that the _kernel_ name for `poly.f` is `poly.poly.f` while
the printing name would be `poly.f`. For `poly.g`, we have kernel name
`polyext.poly.g` while the printed name is `poly.g`. Kernel names and
user names are hence entirely decorelated.

```coq
(in polyext.v,)

Namespace poly.
  Use poly.

  Namespace .notations. (* Refered to by poly.notations*)
	Notation foo "R [ x ]" := ...
  End .notations.
  Put .notations in poly. (* No global notation namespace, these
	notations can only be used if poly is used *)
	
	
  (* Here we can use "R [ x ]" *)
End poly.

Notation bla "blabla" := .. (* In the polyext namespace *)

Remove bla. (* Still accessible as [polyext.bla] but removed from
  user-accessible names *)

(* Here "R [ x ]" is hidden *)
```

```coq
Require poly.
Use poly - poly.notations. (* remove notations *)



Local Namespace Init.
  Module Datatypes.
    ...
  End Datatypes.
End Init.
```

# Semantics

Roughly, `Namespace`s act like directories in the file tree.

- Any object `Z` inside a (global) namespace `A.B...C` shall have the kernel name `A.B...C.Z`.
- Any object `Z` inside a (local) namespace `A.B...C` in a module/namespace `F.G...H` shall have the kernel name `F.G...H.A.B...C.Z`.
- A `Global Namespace` command shall be exactly the same as an unqualified `Namespace` command
- `Namespace`s, unlike `Section`s and `Module`s, *must* permit duplication; it is not an error to write
```coq
Namespace Coq.Init.
  Module A.
  End A.
End Coq.Init.
Namespace Coq.Program.
  Module B.
    Require Import Coq.Init.A.
  End B.
End Coq.Program
Namespace Coq.Init.
  Module C.
    Require Import Coq.Program.B.
  End C.
End Coq.Init.
```
- Nesting global `Namespace`s shall emit a warning like `Warning: Namespaces declare absolute prefixes.  The innermost namespace dominates any outer namespace.  Use Local Namespace to nest namespaces, or use Global Namespace to suppress this warning.`
- It shall be an error to use a `Namespace` inside a Functor, a `Module Type`, a `Section`, or a `Proof`.  It shall not be an error to use `Namespace` inside an unparameterized `Module`.
- At the disgression of the implementor, objects inside `Namespace`s may be restricted to `Module`s, or to any set of objects which include `Module`s.

# Consistency Concerns

The kernel must check that no two objects (`Module`s, `Definition`s, etc) have the same kernel name, and issue an `inconsistent assumptions over` error message if declaring a definition or importing a file yields two objects with the same kernel name.
As a side-effect, this should robustly guard against the recent proofs of `False` discovered by [Maxime Dénès](https://github.com/maximedenes).

## Changes
* 2016-07-10, first draft
