https://hdlbits.01xz.net/

https://www.chipverify.com/systemverilog/systemverilog-simple-testbench

### Basic

##### Data Values

* 0/1
* x: unknown
* z: high-impedance, not connected

##### Data Types

* wire
* reg
* integer
* time (64-bits)
* real

```verilog
// in and out are wires
// connection (single direction), not wire itself
module top_module( input (wire) in, output out );
	assign out = in;
endmodule

// bitwise not and or
assign out = ~in;
assign out = a & b;
assign out = a | b;
assign out = a^b // xor
```

### Vectors

```verilog
// vector type [upper:lower] vector_name;
input wire [3:0] a;
reg [7:0] mem [255:0]; // 256 * 8-bit registers

// prevent implicit nets created by assign statements
`default_nettype none

// logical operation ! && || ^
assign out = !a;

// concatenation
assign {out[7:0], out[15:8]} = in;
assign out = {in[7:0], out[15:8]};

// concatenation operator {num{vector}}
assign out = { {24{in[7]}}, in };
```

### Module

```verilog
// connect by position
mod_a instance1(wa, wb, wc);
// connec by name
mod_a instance2(.out(wc), .in1(wa), .in2(wb));
```

### Procedures

```verilog
// Computational: always @(*), equivalent to assign
// Clocked: always @(posedge clk), 
assign out = a & b | c ^ d;
always @(*) out = a & b | c ^ d; // out must be a reg

// procedural assignment can only be used inside a procudure
x = y // blocking, executed one by one
x <= y // non-blocking, used in clocked always block, simultaneously
always @(posedge clk) begin
	out_always_ff <= a ^ b;
end

// if-else
// Remember to use else statement to prevent if statement latches
// Warning (10240): ... inferring latch(es)
assign out_assign= (sel_b1==1'b1 && sel_b2==1'b1)? b:a;

always @(*)begin
	if (sel_b1==1'b1 && sel_b2==1'b1) begin
		out_always  = b;
	end
	else begin
		out_always = a;
	end
end

// case
always @(*) begin
	case(sel)
		1'b0: out = data[0];
		1'b1: out = data[1];
	endcase
end

// value z represent don't care
// each statement is checked sequentially
always @(*) begin
    casez (in[3:0])
        4'bzzz1: out = 0;   // in[3:1] can be anything
        4'bzz1z: out = 1;
        4'bz1zz: out = 2;
        4'b1zzz: out = 3;
        default: out = 0;
    endcase
end

// When all outputs must be assigned a value in all possible conditions
// To preven latch, we can assign "default value" before case statement
always @(*) begin
    up = 1'b0; down = 1'b0; left = 1'b0; right = 1'b0;
    case (scancode)
        ... // Set to 1 as necessary.
    endcase
end
```

### More Verilog Features

```verilog
// reduction
assign out = & a[2:0];
assign out = a[2] & a[1] & a[0];

// Combinational for loop
integer i;
reg [7:0] counter;
always @ (in) begin
	counter = 0;
	for (i=0; i<255; i=i+1) begin
		if (in[i]==1'b1) counter = counter+1'b1;
	end
	out = counter;
end

// use generate to create multiple instances of modules
genvar i; 
one_bit_FA FA1(a[0],b[0],cin,cout[0],sum[0]);
generate
    for (i=1; i<100; i=i+1) begin : Full_adder_block
        one_bit_FA FA(a[i],b[i],cout[i-1],cout[i],sum[i]);
    end
endgenerate
```

### Testbench

![[Pasted image 20230529162117.png]]

* Generator: generate different input stimulus
* Interface: contain design signals that can be driven or monitored
* Driver: drive the generated stimulus to the design
* Monitor: monitor the input-output ports
* Scoreboard: checks output from the design
* DUT: design under test

```verilog
`timescale 1ns/100ps

module tb;
	reg clk;
	reg sig;
	// Clock generation, process starts at time 0ns
	always #5 clk = ~clk;
	// Initial block: process starts at time 0ns
	initial begin
		$monitor("Time = %0t clk = %0d sig = %0s", $time, clk, sig);
		sig = 0;
		#10 clk = 0;
		#10 sig = 1;
		#10 sig = 0;
		#10 $finish;
	end
endmodule
```
