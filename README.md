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


6. Add constrints.xdc file

![image](https://user-images.githubusercontent.com/85632581/171061580-43f008d5-71a8-400c-b037-a02bb128e58b.png)


7. Run Synthesis
8. Run Implementation
![image](https://user-images.githubusercontent.com/85632581/171062074-5b3858e7-6ba2-49e1-acb2-ed4cac19d6a9.png)

Once completing the implementation,
9. Check for setup and hold timing violation. 

Open the implemented design

![image](https://user-images.githubusercontent.com/85632581/171062200-c6b5c7a7-4a57-4e4a-a244-c1b867c8afe0.png)

The next image show the truth and false paths:

![image](https://user-images.githubusercontent.com/85632581/171062438-fdc2bcb4-eaa2-4cc6-bb0c-3474b039be2d.png)

Green: Truth, Red: false.

Observation: Faced hold violation in false path.

Solution: cretae false path constrains for paths 71 and 72:

`set_false_path -hold from [get_pins ] to [get_pins ]`

`set_false_path -hold -from [get_pins uut1/inst/plle2_adv_inst/CLKOUT0] -to [get_pins uut3/inst/ila_core_inst/*/D]`

`set_false_path -hold -from [get_pins uut1/inst/plle2_adv_inst/CLKOUT0] -to [get_pins uut3/inst/ila_core_inst/u_trig/U_TM/N_DDR_MODE.G_NMU[2].U_M/allx_typeA_match_detection.ltlib_v1_0_0_allx_typeA_inst/probeDelay1_reg[0]/D]`


After adding the constraints a new run of synthesis and implementation is done.

This time there is no timing violations.

![image](https://user-images.githubusercontent.com/85632581/171065383-0373f7b3-2ba5-4424-8e22-17da65183b79.png)


10. Generate the bitstream and program the FPGA:
![image](https://user-images.githubusercontent.com/85632581/171065533-6babeed2-e89a-4eef-ad9a-1ab708d1018a.png)

To program the FPGA and see the ILA we need to have a board available.

## Open question: 

How to probe the core_clk on ILA and verify that frequency of core_clk is 100 MHz?

Answer: The frequency of the clock used for the ILA must be at least equal (ideally higher) than the frequency of the signals the ILA is monitoring. In this case, the clock of the ILA is the main_clockwhich is three times slower than core_clk. So, to verify that core clock is Ok, core_clock could be used as the reference clock for ILA. Alternatively, the PLL could have an extra output of 200 MHz for instance and use it for the ILA clock.



