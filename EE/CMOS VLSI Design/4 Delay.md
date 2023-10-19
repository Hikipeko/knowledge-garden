### 4.1 Introduction

#### 4.1.1 Definitions

* **Propagation delay time** $t_{pd}$ is the maximum time from input crossing 50% to output crossing 50%.
* **Contamination delay time** $t_{cd}$ is the minimum time from input crossing 50% to output crossing 50%.
* **Rise time**, $t_r$ = time for a waveform to rise from 20% to 80% of its steady-state value.
* **Fall time**, $t_f$ = time for a waveform to fall from 80% to 20% of its steady-state value.
* **Edge rate**, $t_{rf} = (t_r + t_f )/2$.

![[Pasted image 20231016220807.png|350]]![[Pasted image 20231016221332.png|300]]

* The gate that charges or discharges a node is called the **driver** and the gates and wire being driven are called the **load**.
* A [[12 Timing|time analysis]] computes the arrival times, i.e., the latest time at which each node in a block of logic will switch.

#### 4.1.2 Timing Optimization

Good microarchitecture -> good logic -> good transistors.

### 4.2 Transistor Response

* The circuit takes time to switch as the capacitance need to charge/discharge
* Need to solve differential equations, which is complicated.

### 4.3 RC Delay Model

Approximate the nonlinear transistor characteristics with an average resistance and capacitance over the switching range of the gate.

#### 4.3.1 Effective Resistance

* **Effective resistance** is the ratio of $V_{ds}$ to $I_{ds}$ averaged across the switching interval of interest.
* A unit nMOS transistor is defined to have effective resistance $R$.

#### 4.3.2 Gate and Diffusion Capacitance

Assume the contacted source or drain of a unit transistor to have capacitance of about $C$.

#### 4.3.3 Equivalent RC Circuits

![[Pasted image 20231017195019.png|250]]![[Pasted image 20231017195115.png|450]]

#### 4.3.4 Transient Response

* $t_{pd} = RC$

#### 4.3.5 Elmore Delay

* Represent circuits of interest as **RC tree**.
* The **Elmore delay** model estimates the delay from a source switching to one of the leaf nodes changing as the sum over each node $i$ of the capacitance $C_i$ on the node, multiplied by the effective resistance $R_{is}$ on the **shared path** from the source to the node and the leaf.
* $t_{ps} = \sum_i R_{is}C_i$
* Normalized delay $d = t_{pd} / \tau$, where $\tau = 3RC$ for a ideal inverter.
* The delay of a fanout-of-$h$ inverter has delay $d = h + 1$.
* The **parasitic delay** is the time for a gate to drive its own internal diffusion capacitance.

#### 4.3.6 Layout Dependence of Capacitance

* In good layout, diffusion nodes are shared wherever possible to reduce the diffusion capacitance.

