### 1 Course Introduction

* THE, THU 10:00-11:40 DZY 4-503
* https://jicanvas.com/courses/348/assignments/syllabus
* Textbook: Computer Architecture: A Quantitative Approach, by John L. Hennessy and David A. Patterson (Sixth Edition)
* Labs: ~5, ~36 h
* > 16 h/week
##### Grade

* Participation (class, lab, piazza) 5%
* Homework 10%
* Labs 30%
* Final Project & Report 35%
* Final Exam 20%

### 1 Introduction to Computer Architecture

Computer architecture is the SW/HW interface, it's about:

* Instruction set
* Memory management and protection
* Interrupts and traps
* Floating-point standard
* Exploit fabrication technology to meet market demands
* Balance between power, cost, area, performance, security, reliability, etc.

![[Pasted image 20230526115505.png|250]]

##### Trending Architectures

* From sequential to parallel (instruction-level, multithreading, multicore)
* From horizontal to vertical
* From homogeneous to heterogeneous (CPU + GPU + Neural Engine)
* From single die to multi-die

### 2 Quantitative Design and Analysis

#### ISAs

RISC-V, ARMv8, x86, etc.

RISC: small number of fixed-length single-cycle instructions, heavy use of RAM

##### RISC-V

* Memory addressing: byte addressed, aligned accesses faster
* Addressing modes: register, immediate, displacement
* Size of operands: 8-bit, 32-bit, 64-bit

#### Performance

* Latency: time to finish a fixed task
* Throughput: number of tasks in fixed time
* MIPS (millions of instructions per second) = f / CPI

CPU Time = IC (instruction count) * CPI (cycle per instruction) * f (clock frequency)

#### Power and Energy

$$
P = \alpha \cdot f \cdot  C_L\cdot V_{DD}^2 + f\cdot I{\text{peak}}\cdot V_{DD} + V_{DD} \cdot I_{\text{static}} 
$$
Dynamic power + short-circuit power + static power.

##### Power Wall

* 130W of heat is the limit of what can be cooled by air
* Liquid cooling is required above the power level
* Instead of increase f, use multiple cores

##### Metrics

* TDP (thermal design power), amount of heat expected to dissipate to prevent overheating.
* PDP (power-delay product) = Pavg * t, average energy per switching event
* EDP (energy-delay product) = PDP * t, prevent trading increased delay for lower energy
* EDDP (energy-delay^2 product) = EDP * t = Pavg * t^3 ~ CVVf * t^3 ~ C since f ~ V
* EPI (energy per instruction)
* MIPS/watt power efficiency

#### Cost

#### Reliability



### 3 Fundamental Processors

![[Pasted image 20230603172423.png]]

* Pipelining doesn't help latency of single task, it helps throughput of entire workload.
* RISC-V is perfect for pipelining.
* Balanced pipeline is very important.
* Add pipeline registers for isolation.
* Each stage begin by reading values from latch, ends by writing values to latch.
* Balancing pipeline: merge multiple stages & subdivide a stage.
* Recent trends: deeper pipelines & multiple pipelines

##### Number of Stages

* Clocks have skews (variation of pulse time) and jitters (variation of duration)
* T: total instruction execution
* S: number of segments
* b: probability of break
* C: clock overhead
$$
\begin{align}
T/S &\geq \max(P_{\max i}) \\
\Delta t &= T/S + C \\
\text{performance} &= 1/(1+(S-1)b) \quad\text{[IPC]}\\
\text{throughput} &= \text{performance} / \Delta t \quad \text{[IPS]}\\
S_{opt} &= \sqrt{\frac{(1-b)T}{bC}}
\end{align}
$$
* Use longer pipeline if C is low and b is low.
* Control also has overhead.

#### Hazard

* Caused by data dependency.
* Structural (same resource needed for different purposes at the same time)

##### Data Hazards

* Instruction output needed before it's available
* Static (nop) vs dynamic
* Interlock/stall vs forward
* Detection: compare regA & regB with DestReg of preceding instructions

##### Forwarding

![[Pasted image 20230605102916.png|500]]

* P: reading a value that is current being written
* S: just negate register file clock
* Fails when data is from load instruction, need stall

##### Control Hazards

* Static prediction vs dynamic prediction
* Dynamic: use branch prediction buffer, indexed by recent branch instruction addresses


