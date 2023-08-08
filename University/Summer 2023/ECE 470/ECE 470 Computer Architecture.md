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

* RISC-V, ARMv8, x86, etc.
* RISC: small number of fixed-length single-cycle instructions, heavy use of RAM

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



### 4 Advanced Processor

* **Instruction-level parallelism (ILP)**: execute multiple independent instructions fully in parallel.
* I: Superscalar execution of degree P
* II: Out-of-order 

#### In-Order

* **Increasing Superscalar Fetch Rate**
	* Over-fetch and buffer using a queue
	* Loop steam detector: put entire loop in a LSD
	* Trace cache: pack multiple non-contiguous blocks into one contiguous trace cache line
* **Challenges**
	* Order $N^2 \cdot P$ of bypass paths
	* Solution: clustering, group ALUs into K clusters
		* $N/K + 1$ input at each mux
		* $(N/K)^2$ bypass paths in each cluster
	* Register file clustering, reduce read port
#### Out-of-order

* Fetched in compiler-generated order
* Completed (Commit) in order
* Might be executed in some other order
* Instructions are dynamically scheduled by the hardware
* Hardware extracts more ILP by on-the-fly reordering

#### Dynamically-Scheduled Superscalar

![[Pasted image 20230802144606.png|500]]

##### Renaming

* Use a `map_table[arch_reg] -> phys_reg` to map from architectural register to physical register
* Use a free list of physical register for allocation
* **Operations**
	* At decode stage, map input registers, record the old output physical register, and map the output register to a free physical register
	* At commit stage, free the old physical register
* Removes false dependences and leaves true dependences intact!

##### Dynamic Scheduling

* Use a `ready_table[phys_reg] -> yes/no` to indicate if a physical register for an instruction is ready.
* For an instruction with latency of N, set "ready" bit for output register N-1 cycles in the future.

##### Dispatch

* Put renamed instructions into OoO structures
* **Operations**
	* Allocate Issue Queue (IQ) slot if not full, use `issue_queue`
	* Read ready bits of inputs
	* Clear ready bit of output in table

##### OoO Pipeline

* Select oldest of ready instructions
* Wakeup dependents, search for destination in inputs, set ready bit, and free up space in `issue_queue`

##### Commit

* **Re-Order Buffer** holds all info for recovery and commit (instruction, order, arch_reg, phys_reg)
* Update register file
* Check if branches have been mis-predicted

#### Memory Dependencies

##### Store

* `ld[r2] -> r1; st r4 -> [r3]`, what if r2 is equal to r3?
* Solution: write into cache only when certain  (at commit stage)

###### Store Queue (SQ)

![[Pasted image 20230802160552.png|250]]

* Allows for recovery of speculative stores
* Allows for out-of-order stores
* At dispatch, each store is given a slot in SQ
* Store writes to SQ, not M in the execution stage.
* Add age fields to SQ indicating the correct order.
* Memory forwarding: load searches the SQ, solving RAW for memory
	* On load, select from SQ the youngest store that 1. has the same address and 2. is order than the load.
* Problem: the load doesn't know address until execution!
	* Solution 1: conservative load scheduling: load after all older stores have executed (low ILP)
	* Solution 2: speculate and recover (squash offending load and all newer instructions)
		* Sometimes we load an outdated value which should be the result of a load.
		* Use LQ to detect this

###### Load Queue (LQ)

![[Pasted image 20230802162234.png|250]]

* Detect load ordering violations (write after read)
* Load execution: write address into LQ
* Store execution: search LQ, younger load with same address? Squash!
* **SQ + LQ**
	* Allows aggressive load scheduling
	* Stores don't contain load execution

### 5 Memories

| SRAM                                        | DRAM                                                 |
| ------------------------------------------- | ---------------------------------------------------- |
| Static, holds data when power is maintained | Dynamic, must be refreshed periodically to hold data |
| Require multiple transistors (typically 6)  | Require only one transistor                          |
| Fast                                        | Cheap                                                |
| Cache                                       | Main memory                                          |

* Flash Memory (non-volatie)
* Bank -> Device -> Rank, increase DRAM bandwidth
* **Memory Wall**: memory access is slower and slower in terms of clock cycle
	* Row buffer: buffer recently accessed data
	* Double data rate (DDR): transfer data on the rising and falling clock edges
	* DRAM banking: increase the number of parallel banks
	* Stacked DRAMs

#### Cache

* Associativity
* Block size
* Capacity

#### Advanced Cache Optimization & Memory Management

##### Victim Buffer (VB)

* Small fully-associative cache
* Small, so very fast
* Block kicked out of L1 cache placed in VB

##### Hardware Prefetching

* Bring data into cache proactively
* Relies on utilizing memory bandwidth that otherwise would be unused

##### Compiler Optimizations

* Reorder procedures in memory to reduce conflict misses
* Loop interchange: change nesting of loops to access data in memory order
* Blocking, loop, loop fusion, merging, splitting of arrays

##### Write Propagation

* Write-through: immediately send the write to the next level
	* Write non-allocate: on write, don't allocate new cache line
* Write-back: postpone write block back to memory as late as possible
	* Allocate cache block on miss
	* Update cache block and then memory block

##### Write Buffer

* Write-through
	* CPU write data into write buffer
	* Memory controller write contents of the buffer to memory
	* CPU proceeds to next step
* Write-back (WBB) on cache miss
	1. Send fill request to next level
	2. Write dirty block to buffer
	3. When new blocks arrives, put it into cache
	4. Write buffer contents to next-level

##### Store Buffer

* Buffer store from processor to cache

##### Reducing Miss Penalty

* Early Restart: start as soon as the requested word of the block arrives
* Critical word first: request the missed word first
* Merging write buffer: write back to memory with a fuller write buffer slot
* Non-blocking cache (look-up free cache): allow data cache to continue to supply cache hits during a miss
	* **Miss Status Holding Register**
	* On a miss, allocate a MSHR entry to track the request
	* Allowing fetched data to be immediately forwarded to the appropriate destination
* Pipelined access: pipeline cache access to improve bandwidth
* Multi-banked Caches: divide the cache into independent banks

##### Reduce Hit Time

* Small and simple first-level caches
* Way prediction: predict which way it is in
* Increase memory bandwidth
	* Store tags in the HBM
	* Place the tags and the data in the same row in the HBM
