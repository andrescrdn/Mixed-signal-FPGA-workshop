# Mixed-signal-FPGA-workshop
## RISC-V MYTH (Digital) + PLL (Analog)
PLL: Output = Multiplier of 8x the input frequency\
PLL output clock connected to the rvmyth core.

## RVMYTH RISC-V Core
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

Block design of the system to implement on Vivado:

![image](https://user-images.githubusercontent.com/85632581/171057877-8ac95f19-5dcb-4795-a99c-9dc503d1eaa2.png)


Steps:

1. Create the project targeting Zedboard hardware

![image](https://user-images.githubusercontent.com/85632581/171058302-387eba23-2651-40ce-a4d8-e8a1ff5e6131.png)

![image](https://user-images.githubusercontent.com/85632581/171058557-cc7d14a1-a40a-4986-b6ce-86917610e891.png)

2. Add design sources: rvmyth.v, clk_gate.v and top_SoC.v

![image](https://user-images.githubusercontent.com/85632581/171058984-c2f98d70-4005-4176-9b97-c245006115f2.png)


3. Generate PLL and ILA IPs from Xilinx IP catalog

3.1 PLL from IP Catalog, clocking wizzard. Make sure to select Allow Override Mode under PLLE2 Settings and BUF IN for COMPENSATION:
![image](https://user-images.githubusercontent.com/85632581/171059677-0b8ff31f-e4bc-4bb2-89d7-61732bffe0b0.png)

3.2 ILA
![image](https://user-images.githubusercontent.com/85632581/171059877-c4d6e4b6-177e-4540-bf0f-c847b9fe1005.png)

Once the output products are generated the project looks like:

![image](https://user-images.githubusercontent.com/85632581/171060000-dbb0693a-4e19-42cb-8171-b8aee621388c.png)


PLL: used as a multiplier
ILA: used as a waveform viewer

4. Add top_SoC_tb.v as a simulation source:
![image](https://user-images.githubusercontent.com/85632581/171060237-2c263ea4-f1ab-418e-93e6-0caf13c83d12.png)

5. Run behavioral simulation and verify the functionality:

![image](https://user-images.githubusercontent.com/85632581/171060969-6f55376f-e56e-4cd4-ac56-79382eba92cd.png)


![image](https://user-images.githubusercontent.com/85632581/171061157-1db93341-59d6-49e4-8d44-f4fae3302dbe.png)

![image](https://user-images.githubusercontent.com/85632581/171061199-8db46d7e-648d-4954-a2bb-d77f29fa429b.png)



7. 


3. 



