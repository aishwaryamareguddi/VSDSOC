# VSDSOC-VSD-SoC-Design
Hello everyone! I recently attended a 5-day workshop on VLSI System-on-Chip (SoC) design, and I'm excited to share what I've learned. The program, organized by NASSCOM and VSD, was led by Kunal Ghosh, a notable figure in the industry.It focused on exploring the ASIC design flow using OpenLane and the Google-SkyWater collaboration's 130nm process design kit (PDK). Throughout the course, we learned how to create standard cells, navigate through the Physical Design domain, and generate GDSII files, which are essential for chip fabrication. This hands-on experience has equipped us with practical skills and knowledge in modern ASIC design techniques.
## Table of contents 
 1 Inception of the open-source EDA, OpenLANE,Sky130 PDK.
 
 2 Good floorplan vs bad floorplan and introduction to library cell.
 
 3 Design library cell using Magic Layout and ngspice characterization.

 4 Pre-layout timing analysis and importance of good clock tree.

 5 Final steps for RTL2GDS using tritonRoute and openSTA
## Day 1
### Components of chip
 ![WhatsApp Image 2024-05-20 at 22 21 12_adf5e740](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/0abfdd0f-8924-43f9-b9d3-97d51ce94012)

 1 Pad: Pad are crucial for sending the signal into and out of the chip.They acts as interface between the chip and the external world.

 2 Die: It holds the entire integrated circuit, including the pad and core and making up the complete functional unit of the chip.

 3 Core: The core is the central part of the chip where all the digital logic resides.It is the heart of the chip, handling all the processing task and executing the programmed insructions.

 ### Components of open-source digital ASIC design

 ![WhatsApp Image 2024-05-20 at 23 15 43_9d65b88e](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/3b84f871-ee42-44c7-98e2-7093e8996900)

 ### Key Components of Open Source Digital ASIC Design

#### 1. EDA Tools (Electronic Design Automation Tools)
- **Function**: EDA tools are software applications used to design, simulate, verify, and manufacture integrated circuits.
- **Open Source Examples**:
  - **Yosys**: An open-source synthesis tool that converts RTL (Register Transfer Level) code into a gate-level netlist.
  - **OpenLane**: A complete open-source toolchain for ASIC design that integrates tools for synthesis, placement, routing, and verification.
  - **Magic**: An open-source VLSI layout tool used for physical design, layout editing, and DRC (Design Rule Checking).

#### 2. RTL Design (Register Transfer Level Design)
- **Function**: RTL design describes the behavior of a digital circuit in terms of the flow of data between registers and the logical operations performed on that data.
- **Key Aspects**:
  - **HDL (Hardware Description Languages)**: VHDL and Verilog are commonly used to write RTL code.
  - **Simulation**: Tools like Verilator and ModelSim (for mixed open-source/proprietary workflows) are used to simulate the RTL code to verify its correctness before synthesis.
  - **Verification**: Using testbenches and simulation to ensure the RTL design meets the specified requirements.

#### 3. PDK (Process Design Kit)
- **Function**: A PDK is a collection of files and documentation provided by semiconductor foundries that contain information about the manufacturing process. It includes design rules, device models, and libraries necessary for designing and fabricating an integrated circuit.
- **Key Components**:
  - **Design Rules**: Specifications for layout design to ensure manufacturability and reliability of the chip.
  - **SPICE Models**: Detailed models of transistors and other components for circuit simulation.
  - **Standard Cell Libraries**: Pre-designed and validated logic gates and other basic building blocks used in digital circuits.
  - **Examples**: The Google-SkyWater 130nm PDK is an open-source PDK that provides all necessary resources for designing chips using the 130nm technology node.
 
### RTL2GDS Flow
  - ![image](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/9f5cd75a-2741-47ae-8ef6-90501ddec7b4)

1. **Synthesis**:
     This process involves converting the high-level RTL (Register Transfer Level) code into a gate-level netlist using components from the Standard Cell Library (SCL).
2. **Floorplanning and Power Planning**:
     Floorplanning defines the required silicon area and the placement of major functional blocks within the chip. Power planning ensures adequate distribution of power to all parts of the chip.
3. **Placement**:
    This step involves placing the standard cells on the predefined rows within the floorplan. The cells are aligned with the sites to ensure proper connectivity and optimal performance.
  4. **Clock Tree Synthesis (CTS)**:
    CTS creates a clock distribution network that ensures the clock signal reaches all parts of the chip with minimal skew and latency
5. **Routing**:
    This process involves creating the physical interconnections between the placed cells using the available metal layers. Routing ensures that all signals are correctly connected according to the design specifications to implement the interconnects in a way that meets electrical performance and design rules.
6. **Sign-Off**:
    The final stage involves comprehensive verification processes:
     - **DRC (Design Rule Check)**: Ensures the design adheres to the manufacturing process rules.
     - **LVS (Layout Versus Schematic)**: Verifies that the layout matches the original schematic design.
     - **STA (Static Timing Analysis)**: Ensures the design meets the required timing constraints.
  
### OPENLANE

OpenLane is a comprehensive, open-source toolchain designed for the automated design and fabrication of integrated circuits. It streamlines the entire process from RTL (Register Transfer Level) to GDSII (the final file format for chip fabrication), covering all essential steps such as synthesis, floorplanning, placement, routing, and verification. OpenLane supports various foundries and technology nodes, making it versatile for different manufacturing processes. Essentially, it provides a complete, user-friendly workflow for designing chips, simplifying the complex steps involved in creating integrated circuits.

#### openLANE ASIC Design Flow:
![WhatsApp Image 2024-05-20 at 23 41 07_f923b0ed](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/8c762480-8975-4f6d-a7cc-fc02db927261)







  
