https://www.asic-world.com/verilog/design_flow1.html

### Various Stages of ASIC/FPGA

##### Specification

Define what are the important parameters of the system/design that you are planning to design.

##### High Level Design

Define various blocks in the design and how they communicate.

##### Low Level Design

Describes how each block is implemented. It contains details of State machines, counters, Mux, decoders, internal registers. It is always a good idea to draw waveforms at various interfaces.

##### RTL Coding

Micro design is converted into Verilog/VHDL code, using synthesizable constructs of the language.

##### Simulation

Simulation is the process of verifying the functional characteristics of models at any level of abstraction. We use the waveform output from the simulator to see if the DUT (Device Under Test) is functionally correct.

##### Synthesis

* Synthesis is the process in which synthesis tools like design compiler or Synplify take RTL in Verilog or VHDL, target technology, and constrains as input and maps the RTL to target technology primitives.
* Important thing to note is, synthesis tools are not aware of wire delays, they only know of gate delays.
* After the synthesis there are a couple of things that are normally done before passing the netlist to back end (Place and Route)
	* **Formal Verification :** Check if the RTL to gate mapping is correct.
	* **Scan insertion :** Insert the scan chain in the case of ASIC.c
* High-level synthesis is from high-level design description (e.g. C++) to RTL (e.g. Verilog)
* Logic synthesis if form RTL to netlist (representation of logic gates).

##### Place and Route
* The gatelevel netlist from the synthesis tool is taken and imported into place and route tool in Verilog netlist format.
* Backend team normally dumps out SPEF (standard parasitic exchange format) /RSPF (reduced parasitic exchange format)/DSPF (detailed parasitic exchange format) from layout tools like ASTRO to the frontend team, who then use the read_parasitic command in tools like Prime Time to write out SDF (standard delay format) for gate level simulation purposes.

##### Post Si Validation

Test the real chip.