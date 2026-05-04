# FPGA Internship Screening

## Tools used:

- Icarus Verilog (iverilog)
- GTKWave
- Yosys
- ABC
- SKY130 Standard Cell Library
- GVim / Vim
- Dot Viewer (yosys_show / xdot)
- Oracle VirtualBox
- GitHub

flowchart LR
    RTL --> Simulation --> VCD --> GTKWave --> Verification --> Synthesis --> Netlist --> Validation

# Module 1 – RTL Simulation & Waveform Analysis

## Objective: To understand RTL simulation using Icarus Verilog and GTKWave using 2x1 MUX

### Steps Performed:
- Wrote Verilog module and testbench
- Compiled Using iverilog
- Generated VCD waveform file
- Viewed Waveform in GTKWave

### Results and Observations:
- A behavioral 2:1 MUX is implemented.
- Output y depends on sel signal.
- If sel = 1, output is i1, else i0.
- Testbench initializes inputs and toggles them for verification.

 GTKWave Output
   <p align="center">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.03.jpeg" width="600" title="Simulation Waveform">
</p>
->DESIGN
<img src="Images/WhatsApp Image 2026-05-03 at 22.58.05.jpeg" width="600" title="Simulation Waveform">
</p>

Observation:
- Output y follows input based on sel.
- When sel = 0, y = i0; when sel = 1, y = i1.
- Signals change correctly with time → confirms proper simulation.
- No glitches observed → correct combinational logic.


## Multiple Modules – RTL Desgin
   <p align="center">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.06.jpeg" width="600" title="Simulation Waveform">
</p>


Observation:
- The design uses a hierarchical structure with two submodules.
- sub_module1 performs AND operation: (a & b).
- Output is stored in an intermediate wire (net1) and passed forward
 - sub_module2 performs OR operation with input c.
 - Final output is y = (a & b) | c, showing modular and reusable design.

---

=> Cell Variants and Trade-offs

In digital VLSI design, standard cells are available in multiple variants to provide flexibility in optimizing performance metrics such as delay, power consumption, and silicon area. These variants allow designers to balance trade-offs based on design requirements.

-> Fast Cells:
Fast cells are optimized for minimum propagation delay. They achieve high switching speed by using larger transistors and stronger drive strength. However, this results in increased dynamic and leakage power consumption, as well as larger silicon area.
-> Slow Cells:
Slow cells are designed to minimize power consumption and area. They use smaller transistors with lower drive strength, leading to reduced switching activity and lower leakage. However, this comes at the cost of increased propagation delay.
-> Medium Cells:
Medium cells provide a balanced compromise between speed, power, and area. They are commonly used in non-critical paths where neither extreme performance nor ultra-low power is required.

Thus, the selection of cell variants plays a crucial role in achieving optimal design trade-offs between performance, power efficiency, and chip area.

-> Timing Constraints

Timing constraints are fundamental in synchronous digital circuits to ensure correct data transfer between sequential elements such as flip-flops.

-> Setup Time Constraint

The setup time constraint ensures that input data is stable before the active clock edge. For reliable operation, the clock period must satisfy:

Tclk>Tcq+Tcomb+Tsetup
Where:Tclk:Clock period
Tcq:Clock-to-Q delay of the launching flip-flop
Tcomb:Propagation delay of combinational logic
Tsetup: Setup time requirement of the receiving flip-flop

This condition ensures that data launched from one flip-flop arrives and stabilizes at the input of the next flip-flop before the clock edge.

-> Hold Time Constraint

The hold time constraint ensures that data remains stable for a certain duration after the clock edge. The condition for correct operation is:

Thold​<Tcq​+Tcomb​

Where:
- T hold: Hold time requirement of the receiving flip-flop
This constraint ensures that the data does not change too quickly after the clock edge, preventing incorrect data capture.

## Importance

* Fast cells → Improve performance
* Slow cells → Prevent hold violations
* Balanced selection → Optimal design

=> Key Insight
- Standard cell libraries provide multiple gate variants, enabling designers to optimize trade-offs among performance, power consumption, and silicon area.
- During synthesis, design tools automatically select suitable cell implementations based on specified constraints and optimization objectives (e.g., timing, power, or area).


# Module 2-Logic Synthesis & Optimization

## Objective: Understand different logic designs using yosys and understood different optimizations using multipliers

###

=>  Commands 

The following sequence of commands is used to perform synthesis and generate a gate-level netlist using Yosys:

- yosys
Invokes the Yosys synthesis tool environment.
- read_liberty -lib sky130_fd_sc_hd_tt_025C_1v80.lib
Loads the standard cell library, providing timing and functional characteristics of available cells.
- read_verilog <file.v>
Imports the RTL design described in Verilog.
- synth -top <module_name>
Executes the synthesis process, specifying the top-level module of the design.
- abc -liberty sky130_fd_sc_hd_tt_025C_1v80.lib
Performs technology mapping and logic optimization using the provided standard cell library.
- write_verilog <output_netlist.v>
Exports the synthesized gate-level netlist in Verilog format.
- show
Generates a graphical representation of the synthesized design for visualization and verification
Hierarchical vs. Flat Synthesis
Hierarchical Synthesis

Hierarchical synthesis is a design methodology in which each module of a digital system is synthesized independently while preserving the original design hierarchy. This approach allows designers to maintain structural clarity and modularity throughout the synthesis process.

Each module is processed separately, retaining its boundaries
Results in faster synthesis runtime due to reduced complexity per module
Facilitates easier debugging and verification because of clear module separation
However, optimization across module boundaries is limited, which may impact overall performance
Flat Synthesis

In flat synthesis, the entire design hierarchy is removed, and all modules are combined into a single unified representation before synthesis. This enables global optimization across the entire design.

The design is fully flattened into a single-level netlist
Allows comprehensive optimization across all logic elements
Typically results in improved timing and area efficiency
However, it increases computational complexity, leading to longer synthesis time and more difficult debugging
---

| Parameter    | Hierarchical Synthesis | Flat Synthesis     |
| ------------ | ---------------------- | ------------------ |
| Runtime      | Faster                 | Slower             |
| Optimization | Limited                |       Extensive  |
| Debugging    | Simpler                | More complex       |


### Asynchronous reset
- Reset acts independent of clock
- Whenever reset = 1, output immediately becomes 0
- No need to wait for clock edge
  
 -> WAVEFORM
<p align="center">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.05 (2).jpeg" width="600" title="Simulation Waveform">
</p>
->DESIGN
<p align="center">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.07.jpeg" width="600" title="Simulation Waveform">
</p>

### Asynchronous Set
- Set works independent of clock
- Whenever set = 1, output immediately becomes 1
- No dependency on clock signal
  
 -> WAVEFORM
   <p align="center">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.08 (1).jpeg" width="600" title="Simulation Waveform">
</p>
  ->DESIGN
   <p align="center">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.11.jpeg" width="600" title="Simulation Waveform">
</p>

### Synchronous Reset
- Reset works only on clock edge
- Output changes only at posedge of clock
- If reset = 1, then on next clock:
Output becomes 0

-> WAVEFORM
   <p align="center">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.07 (1).jpeg" width="600" title="Simulation Waveform">
</p>
  ->DESIGN
   <p align="center">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.10 (1).jpeg" width="600" title="Simulation Waveform">
</p>

  
## MULTIPLEXERS 

=> mul2 (y = a * 2)
      <p align="">
  <img src="Images/WhatsApp Image 2026-05-04 at 01.05.48.jpeg" width="600" title="Simulation Waveform">
</p>

Observations:
- RTL defines multiplication: y = a × 2
- After synthesis, it becomes: y = {a, 1'b0}
- This represents a left shift by 1 bit
- No multiplier hardware is required
- Dot diagram shows bit shifting (wiring) instead of arithmetic logic
  
    
 => mul8 (y = a * 8)
   <p align="">
  <img src="Images/WhatsApp Image 2026-05-03 at 22.58.09.jpeg" width="600" title="Simulation Waveform">
</p>
        

Observations:
- RTL defines multiplication: y = a × 8
- After synthesis, it is implemented using bit shifting
- Equivalent to a left shift by 3 bits
- Achieved through simple wiring connections
- No dedicated multiplier hardware is used

  
# Key Learnings

* RTL to Gate-level transformation
* Importance of timing libraries
* Trade-offs in speed, power, and area
* Correct coding practices for synthesis
* Differences between synthesis strategies

   


   





