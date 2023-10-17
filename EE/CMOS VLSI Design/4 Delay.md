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