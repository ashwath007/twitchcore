# twitchcore

A RISC-V core, first in Python, then in Verilog, then on FPGA.

# TODO

* Fix unaligned loads/stores (I think this is good now, at least acceptable)
* Make pipelining work
* Add M instructions for fast multiply and divide
* Add better introspection
* Switch to 64-bit
* Add "RISK" ML accelerator ("K" Standard Extension)

# TODO (later)

* Many ROBs like M1 go very fast

# Notes on Memory system

8 million elements (20MB) = 23-bit address path

We want to support a load/store instruction into 32x32 matrix register (2432 bytes) like this:
* Would be R-Type with rs1 and rs2.
* rs1 contains the 23-bit base address 
* rs2 contains two 24-bit strides for x and y, and two 8-bit masks in the upper bytes (0 is no mask). Several of these bits aren't connected
* "rd" is the extension register to load into / store from

Use some hash function on the addresses to avoid "bank conflicts", can upper bound the fetch time.

# Notes on ALU

matmul/mulacc are the big ones, 65536 FLOPS and 2048 FLOPS respectively

Have to think this through more with the reduce instructions too.

It's okay if the matmul takes multiple cycles I think, but the mulacc would be nice to be one.

matmul
* load with stride 0 in X
* mul
* reduce


# Notes on mini edition in 100T

16x16 registers (608 bytes), 256 FMACs (does it fit)

* 64k elements = 16-bit address path
* 2x12-bit strides, 2x4-bit masks
* Nah, actually switch to 64-bit first



