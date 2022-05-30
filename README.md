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


