# pes_vending_machine

- Creating a Verilog simulation of a parking ticket vending machine that accepts Rs 5 and Rs 10 coins and dispenses a Rs 20 ticket involves designing a digital system that manages coin inputs, validates the coin values, calculates the total amount, and dispenses the ticket when the required amount is reached.
- The machine is implemented using principles of a Finite State Machine, which hops across different states while keeping track of the input coins.

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/1fee65f5-59f3-44df-ab8f-32603cc3c320)

## Tools Used in RTL to GLS flow are:
1. iVerilog:
- A free and open-source Verilog simulation and synthesis tool.
- Part of the Icarus Verilog project.
- Utilized for simulating Verilog hardware description language code.
- It enables testing the design using a test bench, which applies stimulus to verify the functionality.
- Monitors input changes and evaluates corresponding output responses.
2. GTKwave:
- A free and open-source waveform viewer.
- Mainly used for visualizing simulation results produced by digital simulation tools such as Icarus Verilog.
3. Yosys:
- An open-source framework designed for Verilog RTL synthesis.
- Widely employed in digital design for converting high-level digital circuit descriptions into gate-level representations.
- Helps transform behavioral descriptions (e.g., Verilog code) into netlists, offering detailed information about the digital logic through gates and their connections.

## Step 1
- To initiate the flow, the initial step involves creating a Verilog code that encapsulates the idea. This code is saved as a ".v" file. Simultaneously, a testbench is crafted for this file. Both the Verilog code and its corresponding testbench are then loaded into the iVerilog tool. By doing so, the tool can execute the simulation and generate a dump file, allowing us to visualize the waveform.
`vim pes_ptvm.v`

![dut_file](https://github.com/ani171/pes_vending_machine/assets/97838595/3a732948-e7da-4640-bf92-e20d8b3a0228)

`vim pes_ptvm_tb.v`

![tb_file](https://github.com/ani171/pes_vending_machine/assets/97838595/9a2e76a1-66c1-44cd-a517-ea912a079b12)

## Step 2

```
iverilog pes_ptvm.v pes_ptvm_tb.v  
ls
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/33c46aff-c2c3-4ea4-b6c9-13ba77799d5e)

```
./a.out
gtkwave dump.vcd
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/e2130afe-887e-43f1-b222-7c17b044255c)

### Pre-Synthesis result

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/f2f014fb-b17d-4894-8b08-659ca7a6ace5)

## RTL Synthesis
- In RTL synthesis:
  - The RTL design is transformed into a gate-level netlist with designer-specified constraints.
  - The design is converted from abstract RTL to logic gates.
  - The logic gates are mapped to technology-dependent gates from libraries.
  - The mapped netlist is optimized while adhering to designer constraints.
= To perform RTL synthesis:
  - Utilize the Yosys tool to generate a netlist.
  - Run the generated netlist using iverilog, along with the ".net" file and the testbench, to create an executable file "a.out."
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/d2babb18-f720-4d42-8f2a-82f8d8b935ca)

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog pes_ptvm.v
synth -top pes_ptvm
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/9e7ca8de-e1b3-44bd-ad1e-7eb0b085359a)

```
abc -liberty -lib ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![ani171_netlist](https://github.com/ani171/pes_vending_machine/assets/97838595/745f32f0-4cbb-422a-918b-9f6fd1270410)

### To get ".net file"
- In the yosys tool
```
write_verilog pes_ptvm_net.v
!vim pes_ptvm_net.v
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/39df7485-b6b3-459e-8112-731744c5b539)
![1](https://github.com/ani171/pes_vending_machine/assets/97838595/c46badea-c3e4-4462-89a4-b2a971cf1680)


## Gate Level Simulation

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v pes_ptvm_net.v pes_vtvm_tb.v
ls
gtkwave dump.vcd
```
![new](https://github.com/ani171/pes_vending_machine/assets/97838595/a72bb91a-6bd5-46c3-ad92-d1000906653f)
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/6d507b9d-a31c-4de6-a5f1-0bf4c6f876e3)


![image](https://github.com/ani171/pes_vending_machine/assets/97838595/a4c2a3a2-b631-46fb-9f5d-5c026343e62d)

## Physical Design

Physical design is the essential procedure that converts an abstract depiction of an electronic system, like an integrated circuit or computer chip, into a practical layout fit for manufacturing. This intricate process involves a series of stages for organizing and structuring different components, such as transistors, wiring, and connections, on a semiconductor material.

Key facets of physical design encompass:

1. Floorplanning: Defining the spatial arrangement of components and modules within the chip's layout.
2. Placement: Positioning individual elements like transistors and logic gates efficiently on the semiconductor substrate.
3. Routing: Establishing the interconnections or wiring between these components to facilitate data flow.
4. Clock Tree Synthesis (CTS): Structuring the clock distribution network to ensure precise synchronization throughout the chip.
5. Power Planning: Managing power distribution and consumption to maintain optimal operation and minimize energy usage.
6. Signal Integrity Analysis: Assessing the integrity of signals during transmission to prevent interference or distortion.
7. Timing Analysis: Evaluating the timing of signal propagation to meet performance requirements and minimize delays.
8. Design for Testability (DFT): Incorporating features that simplify testing and fault detection during chip production.
9. Physical Verification: Conducting rigorous checks to confirm that the physical design adheres to design rules and is free from errors.
10. Package Design: Creating the external packaging of the chip, considering factors like heat dissipation and connectivity.

### Installation

#### ngspice

- Download the tarball from `https://sourceforge.net/projects/ngspice/files/` to a local directory
```
cd $HOME
sudo apt-get install libxaw7-dev
tar -zxvf ngspice-41.tar.gz
cd ngspice-41
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
sudo make
sudo make install
```
#### magic
```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
sudo make
sudo make install
```
#### OpenLane
```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 
# After reboot
docker run hello-world (should show you the output under 'Example Output' in https://hub.docker.com/_/hello-world)

- To install the PDKs and Tools
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```

### Step 1

- To begin the physical design process, we must create the design file for the "pes_vending_machine" project. This entails having the "pes_vending_machine.v" file and access to the Skywater Process Design Kit (PDK), which contains all the necessary foundry-provided PDK-related files. To accomplish this, we follow these steps within the Openlane design directory:
  1. Create a folder called "pes_vending_machine" within the design directory.
  2. Within the "pes_vending_machine" folder, establish two subfolders named "src" and "config.tcl."

To make the config.tcl file we type the following:
`vim config.tcl`
```
set ::env(DESIGN_NAME) "pes_vending_machine"

set ::env(VERILOG_FILES) "./designs/pes_vending_machine/src/pes_vending_machine.v"

set ::env(CLOCK_PERIOD) "10.000"
set ::env(CLOCK_PORT) "clk"
set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
        source $filename
}
```
- after this, we go to the src file and add the pes_vending_machine.v file that we generated from Yosys in RTL synthesis and the required PDKs for our design.

### Step 2

- To initiate Openlane, we invoke it by executing the following commands
```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane
```
- Once we invoke OpenLane it should look the same as shown below:

![VirtualBox_Pysucal_design_02_11_2023_18_48_28](https://github.com/ani171/pes_vending_machine/assets/97838595/bf25ed7c-4502-49fd-94c4-1534c8c2a2f8)

- After launching OpenLane, the next step is to prepare the specific design we want to work on. In this case, given that we're dealing with a vending machine finite state machine (FSM), we can use the following command to prepare the design
`prep -design pes_vending_machine`

![VirtualBox_Pysucal_design_02_11_2023_18_51_29](https://github.com/ani171/pes_vending_machine/assets/97838595/62318538-2802-46f6-9505-0f0ca7cfcaba)

### Step 3

- after preparing the design we now do the first process of physical design which is `run_synthesis`
- When we execute the "run_synthesis" command in OpenLane, it generates statistics related to the synthesis process, specifically for the "ring_counter." This synthesis operation is marked as successful. During this operation, OpenLane creates a "runs" directory, which contains various logs, results, and reports for the design file that underwent synthesis.
- The run_synthesis statistics are as below

![VirtualBox_Pysucal_design_02_11_2023_18_56_50](https://github.com/ani171/pes_vending_machine/assets/97838595/54cee4ed-075d-40f3-a2aa-5f2ba9ab195b)

![VirtualBox_Pysucal_design_02_11_2023_18_56_59](https://github.com/ani171/pes_vending_machine/assets/97838595/86fb41b6-bcab-48bb-9c53-11f7898ec0cd)

- The runs keep track of the process we do in the openlane as shown below:

![VirtualBox_Pysucal_design_02_11_2023_19_03_28](https://github.com/ani171/pes_vending_machine/assets/97838595/7e3ddddb-a895-424d-b8f6-557975a0aadd)

### Step 4

- Following the synthesis stage, the next step is to create a floorplan for the ring counter. This is achieved by using the "run_floorplan" command. When you run this command, it generates a "floorplan.def" file, which can be utilized to visualize the design using the Magic tool.
- Upon successful completion of the floorplan operation, you will find a file named "pes_vending_machine.floorplan.def" within the results directory, as demonstrated below:

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/22933547-7de0-478e-996b-b4bc5d9051ad)

- This indicates that the "run_floorplan" operation was successful, and now you can utilize the generated floorplan file to view the layout using the Magic tool.
- To view the floorplan layout with the Magic tool, you can use the following command:
`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def ring_counter.floorplan.def &
`
- In this command:
  - -T specifies the tech file.
  - & is used to run the command in the background, preventing prompts from appearing in the terminal.
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/5e034b0f-7642-4644-8d90-9ee1b6892cee)

![VirtualBox_Pysucal_design_02_11_2023_19_17_09](https://github.com/ani171/pes_vending_machine/assets/97838595/fa6e83b4-ebb5-480c-9e2e-a49fc3a12cbd)

![VirtualBox_Pysucal_design_02_11_2023_19_19_40](https://github.com/ani171/pes_vending_machine/assets/97838595/f1c9fc29-f1d4-44fb-b563-fa43ae515eb6)


- To inspect the contents of the generated floorplan file, "pes_vending_machine.floorplan.def," and check for any issues, particularly related to the utilization parameter, we use the following command:
`cat pes_vending_machine.floorplan.def`
- This command allows you to view the contents of the floorplan file, and the output will be displayed in the terminal. You can then analyze the file to identify and address any potential issues with the floorplan definition.

![VirtualBox_Pysucal_design_02_11_2023_19_25_42](https://github.com/ani171/pes_vending_machine/assets/97838595/df9ad4d3-df7d-489d-8b44-88db33143455)

### Floorplan & Placement

- We invoke OoenLane
```
cd OpenLane
make mount
./flow.tcl -design pes_vending_machine
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/eb2a2756-d0eb-40c9-9d8f-7db14d2abe61)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/bdfe5ce9-de08-4c29-80c1-9798bf515990)

- To locate the pes_vending_machine.def file for floorplan we type the following command
```
cd OpenLane/designs/pes_vending_machine/runs/RUN_2023.11.03_05.18.10/results/floorplan
ls
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/4b27d0ce-b621-46c8-ab90-d026c00b9fb5)

- now to view the floorplan we use the following command
```
magic -T /home/bavitha/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read pes_sdw.def &
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/068783c7-23c4-41dd-831b-3b442caf967b)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/461f897e-9795-4685-a6a6-5e7ba32ddeaa)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/07f9e32d-633b-44e9-85a0-22d3095a97b3)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/d24de559-2129-4115-8236-c7f20b1b6fec)

- For placement first change the directory to
```
cd OpenLane/designs/pes_vending_machine/runs/RUN_2023.11.03_05.32.49/results/placement
magic -T /home/anirudh/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read pes_vending_machine.def &
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/5bb21ec7-a2b2-42f3-a96f-a379d9637581)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/2b55eb71-261f-4ca8-b23d-01e36a6a71db)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/7c6c86fd-859a-4480-9c04-dda6e5d661bc)

### Clock tree synthesis

```
run_cts
```

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/90b047ef-5193-45f3-91f4-4c7ffedf2041)

### Routing 

```
magic -T /home/anirudh/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read pes_vending_machine.def &
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/14fab56d-5d08-4abb-9fad-092c83acef50)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/2a46f609-4aa6-41c5-8ef6-ca61b89308d3)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/90c05ce5-62d7-4e2f-8dad-62358bec805e)

### Statistics
- Synthesis

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/7cb7997b-0048-4428-9cfb-5c61bf66aaa7)

- Floorplan

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/69a61583-0537-4d8c-bcf3-7a33ced32811)

- Placement

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/14f4292b-dc08-4278-b767-ecf54102a7d6)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/6be75642-9d59-4159-a02d-4b325638b582)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/4fa4f9d9-8e7a-4742-b8c4-2a886357d279)

- Clock Tree Synthesis

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/d8e66057-d9e8-4144-85c6-a74e544736ac)

- Routing

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/49ac178a-c2fe-485f-886f-532edeb08eb6)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/89e538fb-afff-4ba9-9f91-2cbbaa41fd26)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/04ab3f85-a694-4758-814f-16164f3c76d5)
