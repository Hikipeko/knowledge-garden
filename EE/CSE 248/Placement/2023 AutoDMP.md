### Abstract

Place macros and standard cells concurrently with automated parameter tuning using a multi-objective hyperparameter optimization technique.

### 1 Introduction

* Macros deserve special treatment due to their large effect on the floorplan.
* Mixed-size placement places macros and standard cells simultaneously.
* Macro legalization is challenging.

#### Contributions

* Propose using multi-objective Bayesian optimization to search the design space of macro placement efficiently by tuning the parameters.
* Propose a two-level PPA evaluation scheme to manage the complexity of the search place.
* Propose enhancement to [[2019 DREAMPlace Deep Learning Toolkit-Enabled GPU Acceleration for Modern VLSI Placement|DREAMPlace]] to reduce legalization issues for macros.

### 2 Related Work

* Floorplan: simulated annealing, partitioning, or RL-based methods.
* Simultaneous flows place macros and standard cells together
* Sequential flow (widely used) tentatively place macros and standard cells together, discard the standard cell placement and optimize the macro placement only. Then standard-cell placement with fixed macros is performed.

### 3 Preliminaries

* DREAMPlace cannot guarantee that the placement will be legalized for designs with many macros.
* The placer's parameters and random seeding heavily influence the optimization's convergence.

### 4 AutoDMP Framework

#### 4.1 PPA Proxies

* **Wirelength** Use RSMTs (FLUTE, a fast heuristic)
* **Cell Density** 