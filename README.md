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


## openLANE preparation process
#### Open your terminal and enter the following commands:
1. Navigate to the OpenLane docker directory:
   ```
   cd Desktop/work/tools/openlane_working_dir/openlane/docker
   ```
2. Start the interactive OpenLane flow:
   ```
   flow.tcl -interactive
   ```
   

 ![VirtualBox_vsdworkshop_20_05_2024_12_10_23](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/ce704ffa-db7b-430b-bc36-b3b8d3372472)

 

3. Now that the OpenLane tool is launched and ready, the next step is to load the necessary package. This is done by entering the command `package require openlane`.

   ![VirtualBox_vsdworkshop_21_05_2024_00_22_28](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/eadf5568-eaf8-4f88-998c-b5c693e3fb8d)

4.We will be designing the PicoRV32a CPU. This stage involves setting up the design by merging two LEF files: the cell-level LEF and the technology-level LEF.

To start this process, use the following command:
```
prep -design picorv32a
```

![VirtualBox_vsdworkshop_21_05_2024_00_28_46](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/5d4a79c7-a008-461d-a283-8ea9906d9929)

5.Now, open a new terminal and navigate to the `picorv32a` directory, which contains the `runs` folder. Inside the `runs` directory, you will find a folder named with the date of creation (for example, `20-05_18-58`). Open this folder and look for the `mergef.lef`, `config.tcl`, and `cmds.log` files.

![VirtualBox_vsdworkshop_21_05_2024_00_42_50](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/347a8dc1-e4b8-4890-875e-1857931bfb60)

### Synthesis
Synthesis is the process of transforming a high-level description of a digital circuit, usually written in a hardware description language (HDL) like Verilog, into a gate-level netlist. This netlist details the circuit using logic gates and their connections.

To perform synthesis in OpenLane, go back to the OpenLane preparation setup and execute the command:
```
run_synthesis
```
![VirtualBox_vsdworkshop_21_05_2024_00_34_59](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/c572ba7f-d7c1-4a3e-8b2f-bb4b045f0171)


#### synthesis is done

![VirtualBox_vsdworkshop_21_05_2024_00_53_44](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/9f4958b8-9a1a-4b95-b609-1fa08fa5f2cc)

## Day 2
### Floorplanning
Floorplanning involves organizing the main functional blocks of an integrated circuit (IC) within the chip's layout area. The goal is to optimize performance, area, power consumption, and other design constraints.

### Aspect Ratio
The aspect ratio is the proportion of the height to the width of the netlist. 

After completing the synthesis process, initiate the floorplanning process with the following command:
```
run_floorplan
```

![run_floorplan](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/cd9d06c3-8459-44b8-86e6-181b2b7fbdc4)


floorplan process competed and PDN generation was successful-

![PDN gen](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/e1ba7c51-df03-451f-819e-45bd252b20ce)


open config.tcl file using the command 

      'less config.tcl'
      

![config tcl](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/de83ab25-4cb5-421f-9b5d-6e37dd64ed4d)


To view the contents of the `floorplan.tcl` file present in the configuration directory using the `less` command, you can use:

```sh
less floorplan.tcl
```

![floorplantcl](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/2f2fd2a8-d84d-47cb-8a17-02a70fa8d18e)


To see floorplann design use 'magic' command as below
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![ioplanner](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/8dc9e0d0-a8dd-414c-9e2a-0a4a559ab058) 


ioplacer Designs


![ioplan](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/28c9f44b-5adc-48fc-9113-429921fba781)


![ioplannerwhat](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/091a61d6-fdc5-497b-b6ba-da3cb5b20078)


To view information about a component in logic, place the cursor on the component and press `'s` to select it. Then, in the `tkcon 2.4` main window, enter the cammand 'what'


![floorplan](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/538cecbe-e7ab-4b00-809d-8c1400f212f0)


### Placement

To execute placement, open the OpenLane Preparation Set-Up terminal and enter the command:
```sh
run_placement
```
![run_placement](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/80cf2270-7032-4d15-8756-db86374c731c)

now run_placement process completed with taking screenshot

![placements_run](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/70b14867-a96c-4a12-9348-19af664b4552)

To see placement design use 'magic' command as below
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &'
```
![placement](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/44f0aab8-b37d-473c-9f57-08bc719e8e55)

Magnified images of Magic logic

![placementwhat](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/b79071c5-96f0-44c8-9b52-1de2878cbead

## Day 3
Certainly! Here are the commands formatted properly for each terminal:

### In the first terminal:

```sh
cd Desktop/work/tools/openlane_working_dir/openlane
git clone https://github.com/example/vsdstdcelldesign.git](https://github.com/nickson-jose/vsdstdcelldesign.git
cd vsdstdcelldesign
ls -ltr
pwd
```

### In the second terminal:

```sh
cd Desktop/work/tools/openlane_working_dir/pdks
cd sky130A/libs.tech/magic/
ls -ltr
cp sky130A.tech /home/nickson/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
clear
ls -ltr
magic -T sky130A.tech sky130_inv.mag &

```


![VirtualBox_vsdworkshop_17_05_2024_16_45_33](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/4f11f414-6ad4-41c2-a861-087b9a609fdb)



![invwhat](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/e52cb7e7-e07a-44a3-9565-574e7722e50c)



![lefwrite](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/17e00e9f-9a97-4344-8933-281a6f464d32)



#### "Plot the graph with Voltage on the y-axis and time on the x-axis."

use command as below
```
plot y vs time a
```

![grap](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/8fbc134d-bde7-4934-8356-c1357224cc50)


#### Rise Time : 

* at 20% of value

  ![rise](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/f37f5022-f0cd-4d58-bf15-8423b1a64f25)

* at 80% of value

  ![rise2](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/7a3fadd5-c244-4bc2-a39e-7f52c3827884)

#### Propagation Time

Propagation time is measured at 50% of value

![propa](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/a8c58752-3f8f-4cd2-aa18-4e8015d22da6) 


## Day 4

To continue the process, you need to extract the .lef file from the inverter and incorporate it into the picorv32 design flow. When designing the standard cell, it is crucial to ensure that the input and output ports are placed at the intersections of the vertical and horizontal routing tracks. Additionally, the dimensions of the standard cell should adhere to specific guidelines: the width of the cell must be an odd multiple of the track pitch, and the height must be an odd multiple of the vertical track pitch. These considerations ensure proper alignment and compatibility with the design grid and routing architecture, facilitating seamless integration into the overall design.

open the track.info file

![tracks info](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/0743d08e-3e2f-47c8-9645-b01b4646441b)


To convert the grid into tracks, start by opening the tracks file. Then, in the console window, type the command `help grid` for assistance.

![help grid](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/8f7a36bc-a551-4143-ac58-b3fa44233d69)

Track on layout:

![VirtualBox_vsdworkshop_21_05_2024_12_55_24](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/de09c03f-230f-4106-854b-4782a712e844)

#### convertion of magic layout to std cell LEF

![lefwrite](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/ed69183d-5c4d-4cb8-84fb-7a1cc85a440d)

* Next step is to extract the lef file with following command
  ```
  lef wriite
  ```
  After creating the .lef file, the next step is to integrate it into the picorv32a design. To do this, copy the .lef file into the src folder.

* edit the config.tcl of picorv32a design

  ![5](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/c5a29d7b-1861-4340-a147-406c2d29bd44)

*Done the openLANE flow and run synthesis

![a](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/b3e53d7c-2060-445f-b8a0-25266b13cdc2)


![VirtualBox_vsdworkshop_21_05_2024_19_18_37](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/8c9a6712-1efc-4361-ad59-54ef251bd651)


![VirtualBox_vsdworkshop_21_05_2024_19_19_08](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/1a7b14a3-1363-4fd0-bf9f-4329bc3a0e3c)

* Execute the floorplan by using the command `run_floorplan`.

If you encounter any errors, run the following commands sequentially.
```
init_floorplan

place_io

tap_decap_or
```

![VirtualBox_vsdworkshop_21_05_2024_19_20_00](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/55d95d8e-7d1f-40e8-b7b0-9084da40c083)


![VirtualBox_vsdworkshop_21_05_2024_19_20_15](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/879404fe-596f-4b42-bffa-b9841531753c)


![VirtualBox_vsdworkshop_21_05_2024_19_20_33](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/2c7fd703-4b78-43de-a991-fc88e90267c3) 


![VirtualBox_vsdworkshop_21_05_2024_19_21_33](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/68348247-33d9-4937-a524-d555022875d5)



### Timing Analysis using openSTA and CTS

![VirtualBox_vsdworkshop_21_05_2024_19_30_44](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/e5fcd8b6-ed19-4927-942a-2acc5715279e)


![VirtualBox_vsdworkshop_21_05_2024_19_32_03](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/57231451-40a0-4378-bc93-0297c22f56e7)



![VirtualBox_vsdworkshop_21_05_2024_19_48_44](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/a076ca09-5f04-4bee-9a24-6a3b02dbccfe)


![VirtualBox_vsdworkshop_21_05_2024_19_57_28](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/d110b209-2293-4ac0-b0ff-fd953a2d6c74)

* PDN successful

![VirtualBox_vsdworkshop_21_05_2024_19_58_22](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/dc8a5fc1-a077-437e-9abc-df9740671342)


![VirtualBox_vsdworkshop_21_05_2024_19_57_47](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/325cfff4-3f88-42e1-a276-44935a65f4db)



## Day 5

#### Routing and Final layout

![VirtualBox_vsdworkshop_21_05_2024_19_58_22](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/dbfa948b-be1b-488d-93a5-fac1be7a74d9)

![VirtualBox_vsdworkshop_21_05_2024_20_10_27](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/0a6e3f23-bcaa-40a7-80e8-364c3bf2f6ff)

![VirtualBox_vsdworkshop_21_05_2024_20_12_52](https://github.com/aishwaryamareguddi/VSDSOC/assets/169332867/4144deac-e399-4ccc-8aa9-b7fbdf9bdeac)









 












    


   








  
