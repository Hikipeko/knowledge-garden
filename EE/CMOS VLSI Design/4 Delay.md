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

### 4.4. Linear Delay Model

* Express normalized delay as $d = f + p$, where $p$ is the **parasitic delay** and $f$ is the **effort delay/stage effort**.
* $f = gh$, where $g$ is the *logical effort*, $h$ is the number of driving outputs, aka **electrical effort**.
* Inverter has logical effort 1.
* If the load does not contain identical copies of the gate, the electrical effort is $h = C_{out}/C_{in}$, where $C_{out}$ is the capacitance of the external load being driven.

![[Pasted image 20231019205609.png|230]]

#### 4.4.1 Logical Effort

* **Logical effort** is the ratio of the input capacitance of the gate to the input capacitance of an inverter that can deliver the same output current.
* NAND gates are better than NOR

![[Pasted image 20231019205802.png|500]]

#### 4.4.2 Parasitic Delay

* Delay of the gate when it drives zero load.
* It's rarely advisable to construct a gate with more than four series transistors, or it will lead to large parasitic delay.

![[Pasted image 20231019205947.png|500]]

#### 4.4.3 Delay in a Logic Gate

* Often path delays are expressed in terms of FO4 inverter delays ($5\tau$).

#### 4.4.4 Drive

* It is often more intuitive to characterize gates by their drive, x, rather than their input capacitance.
* Redefine a unit inverter to have one unit of input capacitance, then the drive of an arbitrary gate is $x = C_{in}/g$ (the ability to drive a unit inverter).
* Delay can be expressed in terms of drive as $d = C_{out}/x + p$.

### 4.5 Logical Effort of Paths

Choose the best topology and number of stages of logic for a function.

#### 4.5.1 Delay in Multiple Logical Networks

![[Pasted image 20231019211231.png|500]]

* **Path logical effort** $G = \prod g_i = 1 \times 5/3 \times 4/3 \times 1$
* **Path electrical effort** $H = C_{\text{out(path)}} / C_{\text{in(path)}} = 20 /10$
* **Branching effort** $b = (C_{\text{onpath}} + C_{\text{offpath}})/C_{\text{onpath}}$
* **Path branching effort** $B = \prod b_i$
* **Path effort** $F = GBH$
* **Path delay** is sum of the delays of each stage. $D = \sum d_i = D_F + P$
* **Path effort delay** $D_F = \sum f_i$
* **Path parasitic delay** $P = \sum p_i$
* The product of the stage efforts is $F$. To minimize $D_F$, we choose every $f_i$ to be equal.
* $\hat f = F^{1/N}, \, \hat D = NF_{1/N} + P$
* The minimum delay of the path can be estimated without knowing the size of transistors.
* Also gives us **capacitance transformation** formula to find the best input capacitance for a gate $C_{in_i} = C_{out_i} \cdot g_i / \hat f$
* Increase the size of a a gate may not makes the circuit faster. Only the first time might work.

![[Pasted image 20231019213020.png|300]]![[Pasted image 20231019213033.png|200]]

#### 4.5.2 Choosing the Best Number of Stages

* More gates doesn't mean more delay. Add some inverters can reduce delay.
* A path achieves least delay by using $\hat N = \log_p F$ stages.
* Using a stage effort of 4 is a conventional choice, which is the stage effort of a fanout-of-4 inverter.

#### 4.5.3 Example

#### 4.5.4 Summary and Observations

1. Compute the path effort: $F = GBH$
2. Estimate the best number of stages: $\hat N = \log_4F$
3. Sketch a path using: $\hat N$ stages
4. Estimate the minimum delay: $D = \hat N F^{1/\hat N} + P$
5. Determine the best stage effort: $\hat f = F^{1/\hat N}$
6. Starting at the end, work backward to find size: $C_{in_i} = C_{out_i}\times g_i / \hat f$

### 4.6 Timing Analysis Delay Models





