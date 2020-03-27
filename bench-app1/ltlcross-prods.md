This page explains how the automata in the file `ltlcross-prods.hoa.xz` were generated.

The [`ltlcross` tool](https://spot.lrde.epita.fr/ltlcross.html) is a checker for programs that translates LTL formulas into ω-automata.  `ltlcross` takes as input a list of translators to check, and a list of LTL formulas.  Each formula and its negation is given to each translator, and the resulting automata are compared.  For instance if we have three translators, numered 0, 1, and 2, and one formula φ, each translator will be called on φ to produce an automaton, named respectively P0, P1, and P2, and again each translator will be called on ¬φ to produce an automaton N0, N1, and N2.  Then `ltlcross` will check that Pi×Nj is empty for all pairs of i,j.  Additionally, since in our case the automata are deterministic, `ltlcross` will complement each of those automata, building Comp(P0), Comp(P1), Comp(P2), Comp(N0), Comp(N1), and Comp(N2) to perform additional checks, like making sure that Comp(Ni)×Nj and Comp(Pi)×Pj are empty for any i≠j.  All these automata can use arbitrary acceptance conditions (it's up to the translator tool), and their product will have acceptance conditions that are conjunctions of those acceptance.  The resulting product are tested with the generic emptiness check when they involve Fin terms in their acceptance.

The automata in `ltlcross-prods.hoa.xz` come from such products.  Option `--save-inclusion-products` of `ltlcross` makes it possible to save these products.

We configured `ltlcross` to use four different translators:
   - `ltl2tgba -D -G`  (produces deterministic output with any acceptance condition)
   - `ltl2tgba -D -P`  (produces deterministic output with parity acceptance condition)
   - `ltl2da`  (produces deterministic output with any acceptance condition)
   - `ltl2dpa -s`  (produces deterministic output with parity acceptance condition)
`ltl2tgba` is part of [Spot](https://spot.lrde.epita.fr/), and we used the development version present in this directory, while `ltl2da` and `ltl2dpa` are part of [Owl](https://owl.model.in.tum.de/) and we used version 19.06.03.

The reason to use only deterministic automata is just so that `ltlcross` can easily complement those by dualizing the acceptance condition.

The list of LTL formulas we used is in [this file](https://github.com/atollk/master-thesis/blob/c20fdf3/data/automata/syntcomp/formulas_filtered.txt) and was collected from the SyntComp competition.  So these are not random formulas.

`ltlcross` was run as follows:
```sh
% ltlcross -T60 -F formulas.txt 'ltl2tgba -D -G' 'ltl2tgba -D -P' ltl2da 'ltl2dpa -s' --csv=ltlcross-prods.csv --products=0 --save-inclusion-products=ltlcross-prods.hoa
```

The resulting file was then filtered to remove (1) automata without Fin in the acceptance condition, and (2) automata that are identical up to a renaming of states.

```sh
% autfilt -v --acceptance='Fin-less' ltlcross-prods.hoa | autfilt --uniq | xz -c > ltlcross-prods.hoa.xz
```
