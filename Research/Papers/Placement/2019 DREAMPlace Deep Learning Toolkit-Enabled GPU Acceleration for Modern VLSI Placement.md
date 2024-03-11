---
year: 2019
author: Y Lin, S Dhar, W Li, H Ren, B Khailany, DZ Pan
citation: 174
---
### Abstract

* Cast the analytical placement problem equivalently to training a neural network.
* Achieve over 30x speedup compared to speedup in global placement and legalization without quality degradation compared to [[2018 RePlAce Advancing Solution Quality and Routability Validation in Global Placement|RePlAce]].

### 1 Introduction

* Build an open-source generic analytical placement framework.
* Propose efficient GPU implementations of key kernels in analytical placement.
* Demonstrate over 30x speedup in global placement and legalization without quality degradation.

### 2 Preliminaries

![[Pasted image 20231120120030.png|400]]

* Analytical placement usually has three steps, GP, LG, DP.
* Solve the placement problem following the neural network training procedure, with forward propagation to compute the objective and back ward propagation to calculate the gradient.

![[Pasted image 20231120120451.png|400]]

* Only need to implement the custom operators for wirelength and density cost in C++ and CUDA.
* Optimizer and gradient derivation is provided by the framework (PyTorch).
* Construct a placement framework in Python with low development overhead.

#### 2.3 The ePlace Algorithm

* [[2015 ePlace Electrostatics-Based Placement Using Fast Fourier Transform and Nesterov’s Method#2 Essential Concepts|Weighted-average wirelength]].
* The Solution for the Poisson's equation is as follow. The electric field is defined for each bin.

![[Pasted image 20231120151312.png|400]]

### 3 The DREAMPlace Algorithm

* Starting from random initial placement also achieves the same quality with less runtime.
* Initialization: all the cells are in the center of the layout with a small Gaussian noise.

![[Pasted image 20231120153347.png|400]]

* Pin-level parallelization.
* Optimization Engine: Use Nesterov's method as the gradient-descent solver with a Lipschitz-constant approximation scheme for line search.
* Legalization: develop legalization as an operator in DREAMPlace. On CPU since it's already fast enough.

### 4 Experimental Results

#### 4.1 Placement Acceleration

![[Pasted image 20231120171224.png]]

* GP is 35x speedup.
* LG is 10x faster than the NTUPlace3 legalizer.
* The majority of the runtime is taken by DP, which relies on the external placer currently.

![[Pasted image 20231120181331.png|400]]


[INFO   ] DREAMPlace - iteration  615, wHPWL 7.276104E+07, time 0.332ms