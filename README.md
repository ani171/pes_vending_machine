# pes_vending_machine

- Creating a Verilog simulation of a parking ticket vending machine that accepts Rs 5 and Rs 10 coins and dispenses a Rs 20 ticket involves designing a digital system that manages coin inputs, validates the coin values, calculates the total amount, and dispenses the ticket when the required amount is reached.
- The machine is implemented using principles of Finite State Machine, which hop across different states while keeping track of the input coins.

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


