### 1.1 A Brief History

* 1947 First transistor by John Bardeen and Walter Brattain
* 1958 First IC by Jack Kilby
* 1960 MOSFETs begin to enter production
* 1963 First logic gate using CMOSs (nMOS and pMOS) (much less power than bipolar counterparts)
* 1965 Moore's Law start to become a self-fulfilling prophecy
* 1980 CMOS processes replaced nMOS and bipolar processes

* Although the cost of printing goes down, the one-time design costs are increasing exponentially.
* The best way to learn VLSI design is by doing it. Labs at www.cmosvlsi.com

### 1.3 MOS Transistors

![[Pasted image 20230926212947.png|550]]

* nMOS transistor turns ON when gate voltage is high
* pMOS transistor turns ON when gate voltage is ground
* Assume bulk CMOS
	* Assume the bulk of all nMOS transistors are all connected together (one a same bulk), to the lowest on-chip potential (usually GND).
	* pMOS: we can connect B to S

### 1.4 CMOS Logic

* Boolean algebra represents information using ones and zeros.
* Using the "switch model" of a transistor.
* Example: how to use CMOS to design a NAND gate, avoiding short-circuiting VDD and GND.

##### Static CMOS Design Guidelines

![[Pasted image 20230719104641.png|200]]

|               | pull-up OFF | pull-up ON |
| ------------- | ----------- | ---------- |
| pull-down OFF | Z           | 1          |
| pull-down ON  | 0           | X           |

1. nMOS only pass logic 0s, pMOS only pass logic 1s.
2. Use the above basic structure.
3. Only one of the two networks will be on at once.
4. PUN and PDN are duals of each other
5. The below constructs can only implement inverting functions e.g. $\overline{(A+BC)}$.

![[Pasted image 20230719105003.png|500]]

##### Implementing Arbitrary Function

1. Realize the PDN by inspection.
2. Use deMorgan's theorem to manipulate function to the point where PUN can be implemented by inspection.

![[Pasted image 20230719110115.png|500]]

##### 1.4.6 Pass Transistors and Transmission Gates

* nMOS transistor is an almost perfect switch when passing a 0 and we say that it passes a *strong* 0. However, it passes a *degraded* or *weak* 1.
* pMOS is the opposite
* When MOS used as an imperfect switch, we call it a *pass transistor*
* *Fully restored* logic gate is when nMOS passes 0 and pMOS passes 1.
##### 1.4.7 Tristates

![[Pasted image 20230926221006.png|100]]![[Pasted image 20230926221020.png|300]]

##### 1.4.8 Multiplexers

![[Pasted image 20230926221731.png|300]]![[Pasted image 20230926222056.png|300]]

##### 1.4.9 Sequential Circuits

* Have memory: outputs depend on current and previous input
* **D Latch**: transparent when CLK = 1, opaque when CLK = 0, retaining the previous value
* **Flip-Flops**: copies D to Q on the rising edge of the clock.
* **Register**: a collection of DFF sharing a common clock input.

### 1.5 CMOS Fabrication and Layout

#### 1.5.1 Inverter Cross-Section

![[Pasted image 20230719152849.png]]

#### 1.5.2 Fabrication Process

* The cost of the ship is proportional to the chip area.
* Smaller transistors leads to: faster speed, less cost, less power consumption.
* Masks specify where the components will be manufactured on the chip.

![[Pasted image 20230719153628.png|500]]

![[Pasted image 20230719154301.png|500]]

#### 1.5.3 Layout Design Rules

* **Feature size** is the minimum transistor channel length.
* $\lambda$ is half the feature size.

![[Pasted image 20230927121126.png|550]]

#### 1.5.4 Gate Layouts

![[Pasted image 20230927122014.png|250]]

#### 1.5.5 Stick Diagrams

* Easy to draw since they do not need to be drawn to scale.
* Estimate the height and width by counting metal tracks and multiplying by 8 $\lambda$.

![[Pasted image 20230927123036.png|550]]

### 1.6 Design Partitioning

#### 1.6.1 Design Abstractions

* **Architecture**: x86
* **Microarchitecture**: Pentium 4
* **Logic**: 32-bit adder
* **Circuit**: how transistors are used to implement the logic
* **Physical**

* Digital VLSI design favors the tall, skinny engineer who can evaluate how choices in one part of the system impact other parts of the system.

#### 1.6.2 Structured Design

Regularity, modularity, locality

#### 1.6.3 Behavioral, Structural, and Physical Domains

![[Pasted image 20230927125053.png|450]]

TODO

### 1.7 Example: A Simple MIPS Microprocessor

* 32-bit RISC architecture

![[Pasted image 20230927130044.png|600]]

![[Pasted image 20230927130222.png|500]]

### 1.8 Logic Design

#### 1.8.1 Top-Level Interfaces

![[Pasted image 20230927151304.png|350]]

![[Pasted image 20230927151322.png|500]]

#### 1.8.2 Glock Diagrams

![[Pasted image 20230927151908.png|500]]

#### 1.8.3 Hierarchy

* Decompose complex systems into simpler pieces.

![[Pasted image 20230927152155.png|550]]

#### 1.8.4 Hardware Description Languages

* HDLs provide a way to specify the design at a higher level of abstraction to raise designer productivity.
* Verilog is closer to C, commonly used in SV companies
* VHDL often used by telecommunications companies and defense
* *Logic synthesis* tool is similar to a compiler, which maps HDL code onto a library of *standard cells* to minimize area while meeting some timing constraints.
* Layout can be generated by place & route tools.

### 1.9 Circuit Design

* Arrange transistors to perform a particular logic function
* Estimate the delay and power (using SPICE).
* Time to switch $t = \frac{C}{I}V_{DD}$
* Choose transistor width to meet delay requirements
* Dynamic energy $P_d = CV_{DD}^2f$
* Static energy $P_s = I_s \cdot V_{DD}$
* Get a transistor-level netlist

### 1.10 Physical Design

#### 1.10.1 Floorplanning

* Estimate the area of major units in the chip and defines their relative placements
* Estimate wiring lengths and congestion
* Random logic, datapaths, arrays, analog, and I/O

![[Pasted image 20230927162720.png|500]]

#### 1.10.2 Standard Cells

* Each cell is a multiple of 8 $\lambda$ in width so that it offers an integer number of metal2 tracks
* EDA tools is good enough to map entire design onto standard cells.

#### 1.10.3 Pitch Matching

#### 1.10.4 Slice Plans

### 1.11 Design Verification

* Catch error before manufacturing
* **Testbench** is used for verification, and the expected output is called **test vector**.

### 1.12 Fabrication, Packaging, and Testing

* **Tapeout**: the graphic for the photomask of the circuit is sent to the fabrication

### Summary

* These sections can be found at www.cmosvlsi.com and are labeled with a “Web Enhanced” icon.
* A companion text, Digital VLSI Chip Design with Cadence and Synopsys CAD Tools, covers practical details of using the leading industrial CAD tools to build chips.



