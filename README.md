# pes_vending_machine

- Creating a Verilog simulation of a parking ticket vending machine that accepts Rs 5 and Rs 10 coins and dispenses a Rs 20 ticket involves designing a digital system that manages coin inputs, validates the coin values, calculates the total amount, and dispenses the ticket when the required amount is reached.
- The machine is implemented using principles of Finite State Machine, which hop across different states while keeping track of the input coins.

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/1fee65f5-59f3-44df-ab8f-32603cc3c320)

## To simulate on host

- Use the following commands
```
sudo apt install -y git
sudo apt install iverilog
sudo apt install gtkwave
git clone ani171/pes_vending_machine
iverilog pes_ptvm.v
vvp ./a.out
gtkwave ticketvending.vcd
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/d9a18dc6-9790-4f66-b195-7d7c696b590c)

## Pre-Synthesis

```
git clone https://github.com/ani171/pes_vending_machine
cd pes_vending_machine
iverilog -o pes_ptvm_out.out pes_ptvm.v pes_ptvm_tb.v
./pes_ptvm_out.out
gtkwave pes_ptvm_vcd.vcd
```

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/bff5bb70-d442-4865-a8b3-93563d32d8a9)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/0fd02ebf-defe-4444-8c0a-53135017ccbe)

## Post-synthesis
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog pes_ptvm.v
synth -top pes_ptvm
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
stat
show
write_verilog pes_ptvm_netlist.v
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 ../verilog_model/primitives.v ../verilog_model/sky130_fd_sc_hd.v pes_ptvm_netlist.v pes_ptvm_tb.v
./a.out
gtkwave pes_ptvm_vcd.vcd
```
![image](https://github.com/ani171/pes_vending_machine/assets/97838595/4be4146e-c8bc-4723-86c9-d09e212747b0)

![image](https://github.com/ani171/pes_vending_machine/assets/97838595/5d5cb087-e0da-4dda-9b59-42250a0bca10)

