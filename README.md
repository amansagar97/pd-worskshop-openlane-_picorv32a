# pd-worskshop-openlane-_picorv32a
The following repository consists of steps followed while doing the Advanced Physical Design Using OpenLANE/SKY130 workshop. The workshop focuses on the complete ASIC flow approach from RTL2GDS using open soucrce EDA tools such as OpenLANE/SKY130. RISC-V architechture is followed for designing the the core of PICORV32A.

# Table of Content
  * Introduction to FPGA
  * [About RTL to GDSII Flow](#about-rtl-to-gdsii-flow)
  * [SKYWater130 PDK](#skywater130-pdk)
  * [OpenLANE](#openlane)
  * [Tools Used](#tools-used)
  * [AIM](#aim)
  * [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [How to talk to computers](#how-to-talk-to-computers)
      - [IC Terminologies](#ic-terminologies)
      - [Introduction to RISC-V](#introduction-to-risc-v)
        * [RISC-V Characterstics](#risc-v-characterstics)
      - [Software to Hardware](#software-to-hardware)
        * [What happens when we run a program?](#what-happens-when-we-run-a-program)
        * [How does an application run on a computer?](#how-does-an-application-run-on-a-computer)
    - [SoC design and OpenLane](#soc-design-and-openlane)
       - [Introduction to Digital Design](#introduction-to-digital-design)
         * [What is a PDK?](what-is-a-pdk?)
         * [Environment Setup](#environment-setup)
       - [Simplified RTL to GDSII Flow](#simplified-rtl-to-gdsii-flow)
       - [About OpenLANE](#about-openlane)
    - [Getting familier to open-source EDA tools](#getting-familier-to-open-source-eda-tools)
       - [Contents of the OpenLANE Directory](#contents-of-the-openlane-directory)
       - [LAB Day 1](#lab-day-1)
       - [TASK 1: Finding the d flip flop ratio](#TASK-1-finding-the-d-flip-flop-ratio)
* [Day 2 - Good floorplan vs bad floorplan and introduction to library cells](#day-1---good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
    - [Chip Floor planning](#chip-floor-planning)
       - [Utilization factor and aspect ratio](#utilization-factor-and-aspect-ratio)
       - [Concept of pre-placed cells](#concept-of-pre---placed-cells)
       - [De-coupling capacitors](#de---coupling-capacitors)
       - [Power planning](#power-planning)
       - [Pin Placement and logical cell placement blockage](#pin-placement-and-logical-cell-placement-blockage)
       - [Placement and routing](#placement-and-routing)
       - [LAB Day 2](#lab-day-2)
       - [TASK 2: Calculating area](#task-2-calculating-area)
* [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3---design-library-cell-using-magic-layout-and-ngspice-characterization)
   - [LAB Day 3](#lab-day-3)
   - [Labs for CMOS inverter ngspice simulations](#labs-for-cmos-inverter-ngspice-simulations)
       - [Creating SPICE deck](#creating-spice-deck)
       - [Analysing the inverter](#analysing-the-inverter)
       - [LAB SETUP](#lab-setup)
   - [Inception of Layout Â CMOS fabrication process (16 mask process)](#inception-of-layout-Â-CMOS-fabrication-process-(16-mask-process))
   - [LAB DAY 3 (PART 2)](#labb-day-3(part-2))
   - [LAB DAY 3 (PART 3)](#labb-day-3(part-3))
       - [TASK 3: calculating delays and fall time](#task-3-calculating-delays-and-fall-time)
* [DAY 4 Pre-layout timing analysis and importance of good clock tree](#day-4-pre---layout-timing-analysis-and-importance-of-good-clock-tree)
   - [Pre-layout timing analysis and importance of good clock tree](#pre-layout-timing-analysis-and-importance-of-good-clock-tree)
* [DAY 5 - Final step for RTL2GDS]


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
