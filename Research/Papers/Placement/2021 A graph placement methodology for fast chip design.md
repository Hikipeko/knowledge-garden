### Abstract

* Motivation: floorplanning lack automation.
* A deep reinforcement learning approach to floorplanning.
* Use edge-based GCNN to learn rich and transferable representation of the chip.

### Introduction

![[Pasted image 20240229154153.png]]

* P: how to define action space so that it won't be extremley large?
* To enable transferability, learn representation of chips based on placement quality.
* A nn to predict the reward.
* ==Use AI to speed up the evaluation of design which might take days for commercial EDA tools==

#### Chip floorplanning as a placement problem

* A high-dimensional contextual bandits problem
* Reformulate the problem into a MDP

#### Designing

### Questions

* Place one at a time? how to choose the order? greedy, suboptimal?
* How standard cells are clustered?
* Why place standard cell clusters after macros are decided?
* How does hMETIS work?