# pd-worskshop-openlane-_picorv32a
The following repository consists of steps followed while doing the Advanced Physical Design Using OpenLANE/SKY130 workshop. The workshop focuses on the complete ASIC flow approach from RTL2GDS using open soucrce EDA tools such as OpenLANE/SKY130. RISC-V architechture is followed for designing the the core of PICORV32A.

# Table of Content

* INTRODUCTION
* About RTL to GDSII Flow
* SKYWater130 PDK
* OpenLANE
* Tools Used
* AIM


FPGA, an acronym for Field Programmable Gate Array, is an integrated circuit (IC) that is built with a large number of logical processing resources, or logic blocks. These logic blocks consist of digital logic components such as multiplexers, flip-flops, lookup tables, and adders. We can divide the FPGA in three main parts:-

* Configurable Logic Blocks  — To implement logic functions.
* Programmable Interconnects — To implement routing.
* Programmable I/O Blocks    — To connect with external components.

![image](https://user-images.githubusercontent.com/123876256/215329665-db56fe9b-9421-4aa9-a3ce-df0ba67577e6.png)


We are focusing on the implementation of the logic functions, which comes within the Processor/SoC. 

* Chip Package:

Packages are empty vessels into which the bare chip is placed. Tiny wires connect the leads on the package with the leads on the chip. Pin Location of the package are driven by ARUDINO Board that we are trying to design. The Chip at the center of the package. All the input signals coming from the outside will be trasferred to the chip via the wire connection. Following is the example of QFN 48 PIN Package [7mm X 7mm]:
The Chip sitting inside the package consists of various components.

PADS : Through pads, the signals can be sent inside/outside of the chip. They are intermediate structures connecting internal signals from the core of the integrated circuit to the external pins of the chip package.
DIE : Die is the complete chip which gets manufactured on the silicon wafer. It is the square of silicon containing an integrated circuit that has been cut out of the wafer, on which the given functional circuit is fabricated.
Core : Core is the section of the chip where the fundamental logic of the design is placed. A Core typically consists of following components - SoC, SRAM, ADC/DAC, PLL, SPI.
Foundry IPs : IP = Intellectual Property All the devices are dependent on Foundry. It is a place where the Chip actually get manufactured. Generally, fabrication industries(foundry) provide IPs such as standard cells, IO blocks, etc.. PLL, SRAM, ACD/DAC are some examples of foundry IPs. We communicate with IP with the interface files provided.

MACROS : RISCV SOC, SPI these are the digital blocks which are called Macros.

# About RTL to GDSII Flow
RTL (Register tranfer level) to GDSII (Graphic Data Stream) flow consists of the complete set of steps required to create a file which could be sent for tapeout. The RTL code is synthesized and optimised. After sysnthesis of the code, PnR, floor and power planning is done while keeping in check the timing constraints. At the end GDSII file is written out. The complete flow consists of following steps:

Writting RTL
Synthesis
STA (Static Timing Analysis)
DFT (Design for Testability)
Floorplanning
Placement
CTS (Clock Tree Synthesis)
Routing
GDSII Streaming
# SKYWater130 PDK
It is a Open source PDK (Process Design Kit) which is released by the collabration of Google and SkyWater Technologies foundary. Currently this technology has a target node of 130 nm. It is open to everyone and can be accessed at SkyWater Open Source PDK. This PDK is extremely flexible as it provides many optional featurs as standard features. Hence it povide designers with wide range of design choice.


# Tools Used

* Yosys 	      -    Synthesis of RTL Design
* ABC	          -    Mapping of Netlist
* OpenSTA 	    -    Static Timing Analysis
* OpenROAD	    -    Floorplanning, Placement, CTS, Optimization, Routing
* TritonRoute   -    Detailed Routing
* Magic VLSI	  -   Layout Tool
* NGSPICE	SPICE -    Extraction and Simulation
* SPEF_EXTRACTOR -	  Generation of SPEF file from DEF file
