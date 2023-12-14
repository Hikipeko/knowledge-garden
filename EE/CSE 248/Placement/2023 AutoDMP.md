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
* **Cell Density** Sum of exceeding density of each bin divided by the total cell area.
* **Congestion** Use routing demand estimation technique RUDY.

#### 4.2 DREAMPlace Extensions

* **RUDY with Macro Blockages**
* **SISA Net Weights** Use weighted wirelength to increase correlation with Steiner routing.
* **Gradient Descent Optimizer**
* **Heuristic Simplification**
* **Macro Orientation Refinement**

#### 4.3 Parameter Space

![[Pasted image 20231121122040.png|400]]

* Use target density $d_{\text{target}}$ as both a tunable parameter and a target proxy metric.
* **Default DREAMPlace Parameters** Parameters of the weight schedules of the GD solver are vital for the convergence and quality of the solutions (fifth row).
* **Initial Locations** introduce two parameters (first row).
* **Macro Halos** leave sufficient room in case the macros overlaps after GP.
* **Parameters' Effect**
	* Evaluates the marginal effect of each parameter on the PPA proxies.

![[Pasted image 20231121122458.png|400]]
![[Pasted image 20231121122847.png|400]]

