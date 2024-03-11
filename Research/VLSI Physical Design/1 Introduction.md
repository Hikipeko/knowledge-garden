### 1.1 Electronic Design Automation

* Touches almost every aspect of the IC design flow, from high-level system design to fabrication.
* Address problems at different levels: ICs, multi-chip modules, and printed circuit boards.
* Computer software is used to mostly automate design steps including logic design, simulation, physical design, and verification.

##### Conferences

* Design Automation Conference (DAC)
* Design, Automation and Test in Europe (DATE)
* International Conference on Computer-Aided Design (ICCAD)
* Asia and South Pacific Design Automation Conference (ASP-DAC)

**Design productivity gap**: The number of transistors grows much faster than the number of designers.

##### History

* 1960: The first placer
* 1970s: proprietary
* 1980s: independent EDA software start to appear
* Now: Cadence Design Systems, Mentor Graphics, and Synopsys

### 1.2 VLSI Design Flow

![[Pasted image 20231001173826.png]]

1. **System specification**. Define the overall goals and high-level requirements of the system.
2. **Architectural design**. Make decisions including integration, memory management, cores, internal communication, IP usage, packaging, power, process technology, etc.
3. **Functional and logic design**. Define the functionality and connectivity of each module.
4. **Circuit design**. Although logic synthesis tool converts HDL into gate-level netlist, a number of critical low-level elements must be designed at the transistor level. SPICE.
5. **Physical design**. All design components are instantiated with their geometric representations, returns a set of manufacturing specifications. Affects performance, area, reliability, power, yield (产量)
	1. **Partitioning** breaks up a circuit into smaller modules.
	2. **Floorplanning** determines the shapes and arrangement of modules.
	3. **Power and ground routing** distributes VDD and GND nets throughout the chip
	4. **Placement** finds the spatial location for all cells.
	5. **Clock network synthesis** determines the buffering, gating, and routing of clock signal to meet delay requirements.
	6. **Global routing** allocates routing resources that are used for connection.
	7. **Detailed routing** assigns routes to specific metal layers.
	8. **Timing closure** optimizes circuit performance.
6. **Physical verification**. Verify the layout to ensure correct electrical and logical functionality.
	1. *Design rule checking (DRC)* verifies that the layout meets all technology-imposed constraints
	2. *Layout vs. schematics (LVS)* verifies the functionality of the design
	3. *Antenna rule checking* ensure no antenna effects
	4. *Electrical rule checking (ERC)* verifies the correctness of power and ground connections
7. **Fabrication**. The final layout, usually represented in the *GDSII Stream* format, is sent for manufacturing.
8. **Packaging and testing**.

### 1.3 VLSI Design Styles

##### Full-custom design

* Useful for microprocessors or FPGAs
* High design cost amortized by large production volumes
* Fewest constraints

##### Standard-cell designs

* A predefined cell that has fixed size and functionality, e.g. NAND gate
* Standard-cell based designs, e.g. ASICs
* A substantial initial effort is required to build develop the cell library
* *Over-the-cell (OTC)* use multiple metal layers, and is prevalent in industry

![[Pasted image 20231001203944.png]]

##### Macro cells

* Larger pieces of logic that perform a reusable functionality
* *Top-level assembly* combines pre-designed cells to form the highest hierarchical level of a complex circuit (similar to OOP)

##### Gate arrays

* Silicon chips with standard logic functionality
* Can be mass-produced beforehand
* Gate array-based design has short time-to-market
* The layout is greatly restricted

##### Field-programmable gate arrays (FPGAs)

* Both logic elements and interconnects come prefabricated
* Configurable by users
* *Logic elements (LEs)* are implemented by *lookup tables (LUTs)*
* Interconnect is configured by using *switchboxes (SBs)*
* Flexible, but slower and dissipate more power
* More expensive than ASICs above certain production volumes

![[Pasted image 20231001205304.png]]

##### Structured-ASICs (channel-less gate arrays)

* Cells are usually not configurable

### 1.4 Layout Layers and Design Rules

##### Layout layers

* Silicon: *diffusion layer*
* Polysilicon: *poly*
* Metals: *Metal1, Metal2*, etc.
* *Vias* connect metal layers
* *Contact* connect poly and Metal1
* Wire resistance is given as *sheet resistance* in ohms per square
* Different layers have varying sheet resistance, which strongly affects timing characteristics

![[Pasted image 20231001210024.png]]

##### Design rules

* The design can be fabricated in the specified technology & design will be electronically reliable and feasible
* *Size rules*, such as minimum width
* *Separation rules*, such as minimum separation
* *Overlap rules* such as minimum overlap
* See [[Research/CMOS VLSI Design/1 Introduction#1.5.3 Layout Design Rules|Layout Design Rules]]
* As the size of transistors decreases, the $\lambda$ metric is becoming less meaningful, as some physical properties no longer follow such ideal scaling

##### Physical Design Optimizations

* Express tradeoffs by an *objective function*
* Three types of constraints
	* *Technology constraints* is the constraint by fabrication
	* *Electrical constraints*, e.g. timing constraints for signal delay
	* *Geometry constrains* are introduced to reduce the design complexity
* Difficulties
	* Optimization goals may conflict with each other
	* Constraints lead to discontinuous effects
	* Constraints are tightening
* Rules of thumb
	* Each design style requires its own custom flow (EDA tool)
	* Imposing geometric constrains can greatly ease the problem as the expense of optimization
	* Divide the design process into sequential steps

### 1.6 Algorithms and Complexity

* Use *big-O notation*
* A complexity greater than $O(n^3)$ are often considered *not scalable*
* Nearly all problems are [[Complexity 376#NP-Hard]].

##### Types of algorithms

* *Deterministic*: solving process are repeatable
* *Stochastic*: some decisions are random
* *Constructive*: start with 0 and build up the solution
* *Iterative*: start with an initial solution and optimize repeatedly
* BFS, DFS, Best-first search

![[Pasted image 20231001215638.png]]

##### Solution quality

Heuristic solutions are tested across a suite of *benchmarks*.

### 1.7 Graph Theory Terminology

* A *hypergraph* is a graph where *hyperedges* can connect to a subset of vertices, used to represent multi-pin nets.
* A *multigraph* is a graph that can have more than one edge between two vertices
* A direct graph is *cyclic* if it has at least one directed cycle, otherwise it's *acyclic*. Many EDA problems work on directed acyclic graph (DAG).
* [[Graphs 281#L20 Minimum Spanning Tree|Minimum Spanning Tree]]
* A *rectilinear minimum spanning tree* is a MST where all edge lengths correspond to the rectilinear distance metric.
* The *Steiner tree* is a generalization of spanning tree, allowing connect edges to *Steiner points*

![[Pasted image 20231001220739.png]]
![[Pasted image 20231001221535.png]]

### 1.8 Common EDA Terminology

* *Logic design* is the process of mapping HDL to gate-level netlist.
* *Physical design* is the process of determining the geometrical arrangement of cells and their connections within the IC layout.
* *Component* such as transistors, capacitors
* *Module* is a circuit portion.
* *Block* is a module with a shape.
* *Cell* is a logical or functional unit built from various components, usually gates.
* *Standard cell* is cell with predefined functionality, with library-specific fixed dimension.
* *Macro cell* is cell without pre-defined dimensions.
* *Pin* is used to connect a given component to its external environment.
* *Contact* is the direct connection between silicon and a metal layer.
* *Net/signal* is a set of pins that must be connected.
* *Supply nets* are power (VDD) and ground (GND) nets that provide current to cells.
* *Netlist* is a collection of nets.
* *Pin-oriented* vs *net-oriented*
* *Net weight* are assigned to indicate the net's importance.
* *Connectivity degree/connection cost* is the number of connections between $i$ and $j$.
* *Connectivity* of cell
* *Connectivity graph* is a representation of the netlist as a graph. A $p$-pin net is represent by a complete connection between all the pins.
* *Connectivity matrix* represents the circuit connectivity over $n$ cells
* *Manhattan distance* aka L1 norm

![[Pasted image 20231001223250.png|600]]
![[Pasted image 20231001223834.png|550]]
![[Pasted image 20231001224236.png|500]]
