# pd-worskshop-openlane-_picorv32a
The following repository consists of steps followed while doing the Advanced Physical Design Using OpenLANE/SKY130 workshop. The workshop focuses on the complete ASIC flow approach from RTL2GDS using open soucrce EDA tools such as OpenLANE/SKY130. RISC-V architechture is followed for designing the the core of PICORV32A.

# Table of Content

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
