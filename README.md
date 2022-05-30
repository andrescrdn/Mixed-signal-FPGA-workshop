# Mixed-signal-FPGA-workshop
## RISC-V MITH (Digital) + PLL (Analog)
PLL: Output = Multiplier of 8x the input frequency\
PLL output clock connected to the rvmyth core.

## RVMYTH RICD-V Core
RiSC-V ISA - BAse Ineteger RV32I\
TL-Verilog\
5-stage pipelined processor\
Register File Bypass technique used to mitigate data hazards\
B-Type and J-Type Instructions: extra 2-cycle delay\
RVMYTH currently supports only hte CPU core with a small I memory and D memory\

Github Link: https://github.com/shivanishah269/risc-v-core

Blog: https://www.vlsisystemdesign.com/what-i-did-in-5-day-vsd-workshop-designed-basic-risc-v-core/


## Transaction-Level Verilog (TL-Verilog)
Verilog implementation of TL-X.\
TL-X: Languaje extension defined as a wrapper to any HDL to extend it with transaction level modeling.\
Simplify HDL modeling\
Timing abstract for pipeline = retiming easy. Adding a new pipeline stage is easy: @ 

![image](https://user-images.githubusercontent.com/85632581/171022175-b609ce00-a9be-4537-a03d-ce38aa37b1e0.png)

Flexible: Example WARP-V, different ISAs, pipeline depth, etc.

TL-Verilog details: https://www.redwoodeda.com/tl-verilog

## Why FPGA

Reconfigurable, Prototyping for verification, time to design SoC less then ASIC.

## TL-Verilog to RTL

RISC-V core designed using makerchip: https://www.makerchip.com/

Makerchip desktop application: https://gitlab.com/rweda/makerchip-app

Github link: https://github.com/shivanishah269/vsdfpga

SandPiper is a code generator that generates readable, well-structured, Verilog or SystemVerilog code from the given TL-Verilog code.
We followed the steps to: 
1. Install Sandpiper SaaS (https://pypi.org/project/sandpiper-saas/)
2. `git clone https://github.com/shivanishah269/vsdfpga.git`
3. `cd vsdfpga/verilog`
4. `sandpiper-saas -i rvmyth.tlv -o rvmyth.v --iArgs`

![image](https://user-images.githubusercontent.com/85632581/171029979-e17f3309-f1ab-4fcb-b44a-b06dde432f70.png)

It creates the .v modules, such as the one shown in the image:

![image](https://user-images.githubusercontent.com/85632581/171030353-d57124c1-e9d6-4480-bffd-509095e10aa6.png)

## iVerilog Simulation

Steps:
1. `iverilog rvmyth_pll_tb.v rvmyth_pll.v clk_gate.v`
2. `./a.out`
3. `gtkwave rvmyth_pll.vcd`

![image](https://user-images.githubusercontent.com/85632581/171032112-7d7b525b-38be-4703-890b-cd77f1b8854b.png)


## FPGA create project




