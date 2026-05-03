FPGA Internship Screening – RTL Design, Simulation and Synthesis
Abstract

This project presents a complete flow of digital design starting from Register Transfer Level (RTL) description to gate-level implementation using open-source tools. The work includes Verilog-based RTL design, functional simulation using Icarus Verilog, waveform analysis using GTKWave, logic synthesis using Yosys, and post-synthesis verification.

The objective is to understand how a behavioral hardware description is translated into a physical implementation using standard cell libraries and how correctness is ensured through simulation at different stages. The project is divided into two modules: RTL simulation and waveform analysis, and logic synthesis with timing and optimization concepts.

Tools Used
Icarus Verilog (iverilog) – Verilog compiler and simulator
GTKWave – Waveform viewer
Yosys – Logic synthesis tool
ABC – Technology mapping
Sky130 Standard Cell Library (.lib) – Timing and gate library
GVim / Vim – Code editor
Dot Viewer (xdot / yosys_show) – Netlist visualization
Oracle VirtualBox – Linux environment
GitHub – Version control
Module 1 – RTL Simulation and Waveform Analysis
Objective

To understand RTL simulation using Icarus Verilog and GTKWave by implementing and verifying a 2×1 multiplexer.

Theory
RTL Design

Register Transfer Level (RTL) design describes how data flows between registers and how logical operations are performed. It is written in hardware description languages such as Verilog.

Simulation

Simulation is used to verify the correctness of a design before hardware implementation. It ensures that the logic behaves as expected for given inputs.

Key Components
Design (DUT): Contains the actual logic
Testbench: Generates input stimulus and monitors output
Simulator: Executes Verilog code
VCD File: Stores signal transitions
GTKWave: Displays waveforms
RTL Simulation Flow

Design + Testbench → iverilog → VCD file → GTKWave → Waveform Analysis

Steps Performed
Wrote Verilog module and testbench
Compiled using iverilog
Generated VCD waveform file
Viewed waveform using GTKWave
Observations
A behavioral 2:1 multiplexer is implemented
Output y depends on select signal sel
If sel = 1, output = i1
If sel = 0, output = i0
Testbench toggles inputs for verification
Waveform Analysis
Signals change correctly with time, confirming proper simulation
No glitches observed, indicating correct combinational logic
Output follows expected behavior
Module 2 – Logic Synthesis and Optimization
Objective

To understand logic synthesis using Yosys and study timing concepts, hierarchical design, and optimization techniques.

Theory
Logic Synthesis

Logic synthesis converts RTL code into a gate-level netlist using standard cell libraries.

RTL Code → Gate-Level Netlist

Netlist

A netlist is a structural representation of a circuit that consists of logic gates and their interconnections.

Liberty (.lib) File

A .lib file contains:

Standard logic gates such as AND, OR, NOT
Sequential elements like flip-flops
Timing parameters such as delay, setup time, and hold time
Multiple versions of the same gate with different drive strengths
Standard Cell Variations

Each logic gate exists in multiple variants:

Slow cells: lower power, higher delay
Medium cells: balanced performance
Fast cells: lower delay, higher power

These variations are used to meet timing and power constraints.

Yosys Synthesis Flow

RTL → read_verilog → read_liberty → synth → abc → netlist

Synthesis Steps
Read Verilog design
Load standard cell library (.lib)
Perform synthesis
Map logic using ABC
Generate gate-level netlist
Observations
RTL is converted into a gate-level netlist
Internal wires are created automatically
Behavioral code is replaced by standard cell instances
Technology mapping is performed using library cells
Gate-Level Representation
Design is implemented using real hardware cells
Example: multiplexer mapped to standard cell (sky130_fd_sc_hd__mux2_1)
Structure shows actual hardware connections
Post-Synthesis Verification
Objective

To verify that the synthesized design produces the same output as the RTL design.

Flow

Netlist + Testbench → iverilog → VCD → GTKWave

Result
Gate-level output matches RTL output
Confirms correct synthesis and mapping
Timing and Optimization Concepts
Timing in Digital Circuits

The maximum operating speed of a circuit is limited by the combinational delay between flip-flops.

Clock Constraint

Clock Period ≥ Data Delay + Setup Time

Cell Selection
Fast cells reduce delay but increase power
Slow cells reduce power but increase delay
Timing Issues
Fast paths may cause hold time violations
Slow paths may cause setup time violations
Optimization Strategy
Use a mix of fast and slow cells
Balance performance, power, and area
Faster vs Slower Cells
Type	Advantage	Disadvantage
Fast Cells	High speed	High power
Slow Cells	Low power	High delay
Hierarchical vs Flat Design
Type	Description
Hierarchical	Uses multiple interconnected modules
Flat	Single-level optimized design

Hierarchical design improves modularity and reuse, while flat design improves optimization.

Flop Coding Styles
Use non-blocking assignments (<=) in sequential logic
Avoid incomplete sensitivity lists
Prevent unintended latch inference
Key Insights
Different variants of gates exist to balance speed, power, and area
Synthesis tools automatically choose appropriate cells
Proper RTL coding improves synthesis results
Timing-aware design is essential for correct hardware operation
Results
Successful RTL simulation
Correct waveform generation
Accurate synthesis using Yosys
Matching post-synthesis output
Clear understanding of timing and optimization
Conclusion

This project demonstrates a complete RTL-to-gate-level design flow using open-source tools. It highlights the importance of simulation, synthesis, and verification in digital design and provides a strong foundation for ASIC and FPGA development.

Future Scope
Static Timing Analysis (STA)
Physical Design using OpenROAD
Power analysis and optimization
FPGA implementation and testing
