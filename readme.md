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

## Complete Design Flow

```
RTL Design + Testbench
        │
        ▼
     Iverilog (Simulation)
        │
        ▼
     VCD Output File
        │
        ▼
     GTKWave (Waveform)
        │
        ▼
     Verified RTL Design
        │
        ▼
     Yosys (Synthesis)
        │
        ▼
  Gate-Level Netlist
        │
        ▼
  Netlist Verification
```

# Module 1 – RTL Simulation & Waveform Analysis

## Objective: To understand RTL simulation using Icarus Verilog and GTKWave using 2x1 MUX

### Steps Performed:
- Wrote Verilog module and testbench
- Compiled using iverilog
- Generated VCD waveform file
- Viewed waveform in GTKWave

### Results and Observations:
- A behavioral 2:1 MUX is implemented.
- Output y depends on sel signal.
- If sel = 1, output is i1, else i0.
- Testbench initializes inputs and toggles them for verification.

 GTKWave Output
   <p align="center">
  <img src="WhatsApp Image 2026-05-03 at 22.58.03.jpeg" width="600" title="Simulation Waveform">
</p>
->DESIGN
<img src="WhatsApp Image 2026-05-03 at 22.58.05.jpeg" width="600" title="Simulation Waveform">
</p>

Observation:
- Output y follows input based on sel.
- When sel = 0, y = i0; when sel = 1, y = i1.
- Signals change correctly with time → confirms proper simulation.
- No glitches observed → correct combinational logic.


## Multiple Modules – RTL Desgin
   <p align="center">
  <img src="WhatsApp Image 2026-05-03 at 22.58.06.jpeg" width="600" title="Simulation Waveform">
</p>


Observation:
- The design uses a hierarchical structure with two submodules.
- sub_module1 performs AND operation: (a & b).
- Output is stored in an intermediate wire (net1) and passed forward
 - sub_module2 performs OR operation with input c.
 - Final output is y = (a & b) | c, showing modular and reusable design.

---

## Cell Variants and Trade-offs

| Cell Type | Delay    | Power    | Area     |
| --------- | -------- | -------- | -------- |
| Fast      | Low      | High     | High     |
| Slow      | High     | Low      | Low      |
| Medium    | Balanced | Balanced | Balanced |

---

## Timing Constraints

### Setup Constraint

```
Tclk > Tcq + Tcomb + Tsetup
```

### Hold Constraint

```
Thold < Tcq + Tcomb
```

---

## Importance

* Fast cells → Improve performance
* Slow cells → Prevent hold violations
* Balanced selection → Optimal design

###  Key Insight

* Different “flavours” of gates exist to **balance speed, power, and area**.
* Synthesis tools automatically choose appropriate cells based on optimization goals.

---


# Module 2 - Logic Synthesis & Optimization

## Objective: Understand different logic designs using yosys and understood different optimizations using multipliers

###

### Commands Used:
- yosys
- read_liberty -lib sky130_fd_sc_hd_tt_025C_1v80.lib
- read_verilog <file.v>
- synth -top <module_name>
- abc -liberty sky130_fd_sc_hd_tt_025C_1v80.lib
- write_verilog <output_netlist.v>
- show

# Hierarchical vs Flat Synthesis
## Hierarchical Synthesis
* Module-wise processing
* Faster compilation
* Easier debugging
* Limited optimization

---

## Flat Synthesis

* Entire design flattened
* Better optimization
* Higher runtime

---

## Comparison

| Feature      | Hierarchical | Flat    |
| ------------ | ------------ | ------- |
| Speed        | Fast         | Slow    |
| Optimization | Limited      | High    |
| Debugging    | Easy         | Complex |

---

### Asynchronous reset
- Reset acts independent of clock
- Whenever reset = 1, output immediately becomes 0
- No need to wait for clock edge
  
 -> WAVEFORM
<p align="center">
  <img src="WhatsApp Image 2026-05-03 at 22.58.05 (2).jpeg" width="600" title="Simulation Waveform">
</p>
->DESIGN
<p align="center">
  <img src="WhatsApp Image 2026-05-03 at 22.58.07.jpeg" width="600" title="Simulation Waveform">
</p>

### Asynchronous Set
- Set works independent of clock
- Whenever set = 1, output immediately becomes 1
- No dependency on clock signal
  
 -> WAVEFORM
   <p align="center">
  <img src="WhatsApp Image 2026-05-03 at 22.58.08 (1).jpeg" width="600" title="Simulation Waveform">
</p>
  ->DESIGN
   <p align="center">
  <img src="WhatsApp Image 2026-05-03 at 22.58.11.jpeg" width="600" title="Simulation Waveform">
</p>

### Synchronous Reset
- Reset works only on clock edge
- Output changes only at posedge of clock
- If reset = 1, then on next clock:
Output becomes 0

-> WAVEFORM
   <p align="center">
  <img src="WhatsApp Image 2026-05-03 at 22.58.07 (1).jpeg" width="600" title="Simulation Waveform">
</p>
  ->DESIGN
   <p align="center">
  <img src="WhatsApp Image 2026-05-03 at 22.58.10 (1).jpeg" width="600" title="Simulation Waveform">
</p>

  
## MULTIPLEXERS 

=> mul2 (y = a * 2)
      <p align="">
  <img src="WhatsApp Image 2026-05-04 at 01.05.48.jpeg" width="600" title="Simulation Waveform">
</p>

Observations:
- RTL defines multiplication: y = a × 2
- After synthesis, it becomes: y = {a, 1'b0}
- This represents a left shift by 1 bit
- No multiplier hardware is required
- Dot diagram shows bit shifting (wiring) instead of arithmetic logic
  
    
 => mul8 (y = a * 8)
   <p align="">
  <img src="WhatsApp Image 2026-05-03 at 22.58.09.jpeg" width="600" title="Simulation Waveform">
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

   


   





