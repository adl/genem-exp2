Application 1: Emptiness check of ω-automata in Spot
----------------------------------------------------

This directory contains artifacts for the benchmark of application 1 (emptiness check of ω-automata in Spot) in our FIXME submission titled *FIXME*.

This benchmark was run with the development version of Spot that will eventually become Spot 2.9.  A tarball of this version is given below.  Measurements made in `generic-is-empty.ipynb` and saved in `bench.csv` were done on an Intel Core i7-6820HQ CPU (2.70GHz) running Linux.   Most of the automata generated in the datasets where generated by the aforementioned development version of Spot; automata from `b4.hoa.gz` also used [`ltl2dstar` 0.5.4](https://www.ltl2dstar.de/), and automata from the `ltlcross` dataset also used `ltl2da` and `ltl2dpa -s` from [Owl 19.06.03](https://owl.model.in.tum.de/).

The following files are supplied:
- [`b2.hoa.xz`](b2.hoa.xz): Automata for the `random` dataset
- [`b2b.hoa.xz`](b2b.hoa.xz): Automata for the `random-rep` dataset
- [`b3.hoa.xz`](b3.hoa.xz): Automata for the `Rabin` dataset
- [`b4.hoa.xz`](b4.hoa.xz): Automata for the `Streett` dataset
- [`b5.hoa.xz`](b5.hoa.xz): Automata for the `parity-like` dataset
- [`ltlcross-prods.hoa.xz`](ltlcross-prods.hoa.xz): Automata for the `ltlcross` dataset ([read how this was generated](ltlcross-prods.md))
- [`generic-is-empty.ipynb`](generic-is-empty.ipynb) [![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg?sanitize=true)](https://nbviewer.jupyter.org/github/adl/genem-exp/blob/master/bench-app1/generic-is-empty.ipynb): Jupyter notebook containing Python code for:
  - Python implementation of the `is_empty()` algorithm from the paper (called `is_empty1()` in the notebook)
  - Python implementation of the optimizations described in the *Application 1* section (called `is_empty2()` in the notebook)
  - a version of `is_empty()` that displays a trace of its execution (with intermediate automata), so that one can execute the algorithm on any automaton.
  - code for regenerating the automata from the datasets if the above files are missing
  - code for executing the `is_empty1()`, `is_empty2()`, as well as the C++ version of `is_empty2()` and the previous implementation of that emptiness check, in order to regenerate `bench.csv`
- [`bench.csv`](bench.csv): Result of the experiments, as a CSV file.  Each line correspond to an automaton from one data set.  The following columns are used:
  - `Python/is_empty1`  runtime of `is_empty1()`, in ms
  - `Python/is_empty2`  runtime of `is_empty2()`, in ms
  - `C++/is_empty_spot29`, `C++/is_empty_spot28`, `C++/is_empty_atva19` runtime of the C++ implementations, in ms:
    - `atva19` is the implementation described in our [ATVA'19 paper](https://www.lrde.epita.fr/~adl/dl/adl/baier.19.atva.pdf); it was used in Spot 2.8.7, but previous versions had a typo
    - `spot28` is the implementation of the ATVA'19 paper with a small typos causing unnecessary recursive calls.  It is what was used in our previous benchmark, and it was used in all version between Spot 2.7 and up to Spot 2.8.6
    - `spot29` is the new default implementation for Spot 2.9, with a better theoretical worst case complexity
  - `C++/old_is_empty`  runtime of the former C++ method for generic emptiness check, in ms
  - `/result`  contains `TTTT` if all emptiness check agree the automaton is empty, `FFFF` if they all agree it is non empty
  - `aut/states` number of states in the input automaton
  - `aut/edges` number of edges  in the input automaton
  - `aut/sccs`  number of SCCs in the input automaton
  - `aut/ntsccs`  number of non-trivial SCCs in the input automaton
  - `aut/nrsccs`  number of "non-obviously rejecting" SCCs of the input automaton (an SCC is obviously rejecting if it is trivial or if the checks of lines 7--12 are enough to decide emptiness, in this case no recursion is needed)
  - `aut/nrstates`  number of states in the non-obviously rejecting" SCCs of the input automaton
  - `aut/sets`  number of acceptance sets in the input automaton
  - `aut/acc`  acceptance condition used by the input automaton
  - `/bench`  name of the dataset the automaton belongs to
- [`generic-is-empty-plot.ipynb`](generic-is-empty-plot.ipynb) [![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg?sanitize=true)](https://nbviewer.jupyter.org/github/adl/genem-exp2/blob/master/bench-app1/generic-is-empty-plot.ipynb): Jupyter notebook containing R code for generating tables and figures from `bench.csv`.
- [`spot-2.8.7.dev.tar.gz`](spot-2.8.7.dev.tar.gz): The development version of Spot used for this benchmark.  This will eventually be released as Spot 2.9, and the `is_empty_spot29` implementation is the default.

The easiest way to play with those files is via mybinder [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/adl/genem-exp2/master?urlpath=lab%2Ftree%2Fbench-app1&filepath=bench-app1).
Clicking the previous link will setup a Jupyter instance in the cloud, including a copy of Spot, as well as the necessary Python and R dependencies for the above notebooks.

If you would like to run these notebooks locally on your own installation, please first install the following:
- Make sure you have Python3 with header files (`python-dev` or `libpython3-dev` in most Linux distributions)
- Install the [`scipy`](https://www.scipy.org/install.html) and [`pandas`](https://pandas.pydata.org/pandas-docs/stable/install.html) Python packages.
- [Install jupyter](https://jupyter.org/install)
- Install R
- Start R and run `install.packages(c("tidyverse", "ggplot2", "tables", "tikzDevice", "IRkernel"))`
- If you plan to execute [`generic-is-empty-plot.ipynb`](generic-is-empty-plot.ipynb) to the end, install LaTeX with at least the packages `tikz` (often shipped by distributions in a `texlive-pictures` package) and `preview` (can be called `preview-latex-style` in some distributions).
- Install [`spot-2.8.7.dev.tar.gz`](spot-2.8.7.dev.tar.gz) with  `./configure --disable-devel; make -j4; sudo make install`.
- Then you should be able to start the notebook server with `jupyter notebook`.
