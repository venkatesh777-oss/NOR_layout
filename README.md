
# Completed End to End NOR Gate Design, Synthesis, Layout & Simulation :

This project demonstrates the complete digital design flow of a **2-input NOR gate**, including:

- RTL design in Verilog  
- Synthesis using **Yosys**  
- Functional simulation using **Icarus Verilog**  
- Waveform observation using **GTKWave**  
- Physical layout design using **Magic VLSI**  
- Layout extraction and transistor-level simulation using **Ngspice**

---

## 1. Tools Used

| Tool | Purpose |
|------|---------|
| **Yosys** | RTL synthesis |
| **Icarus Verilog (iverilog + vvp)** | Verilog simulation |
| **GTKWave** | View waveforms |
| **Magic VLSI** | Layout design |
| **Ngspice** | Transistor-level simulation |
| **Ubuntu 20.04/22.04** | Environment |

### Install tools

```bash
sudo apt update
sudo apt install yosys iverilog gtkwave magic ngspice
````

---

## 2. Verilog RTL – NOR Gate

Create file: `nor1_gate.v`

```verilog
module nor1_gate(input a, input b, output y);
    assign y = ~(a | b);
endmodule
```

---

## 3. Testbench for Simulation

Create file: `tb_nor1_gate.v`

```verilog
module tb_nor1_gate();

    reg a, b;
    wire y;

    nor1_gate uut (.a(a), .b(b), .y(y));

    initial begin
        $dumpfile("nor1_gate.vcd");
        $dumpvars(0, tb_nor1_gate);

        a = 0; b = 0;
        #10 a = 0; b = 1;
        #10 a = 1; b = 0;
        #10 a = 1; b = 1;
        #10 $finish;
    end
endmodule
```

---

## 4. Simulation Using Icarus Verilog

Compile:

```bash
iverilog -o nor1_sim tb_nor1_gate.v nor1_gate.v
```

Run:

```bash
vvp nor1_sim
```

Waveform file `nor1_gate.vcd` will be generated.

---

## 5. View Waveform in GTKWave

```bash
gtkwave nor1_gate.vcd &
```

### Expected NOR Truth Table

| A | B | Y |
| - | - | - |
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

---

## 6. Synthesis Using Yosys

Run yosys:

```bash
yosys
```

Execute:

```
read_verilog nor1_gate.v
synth -top nor1_gate
write_verilog nor1_synth.v
```

Generated file: **nor1_synth.v** (gate-level netlist)

---

## 7. Layout Design Using Magic VLSI

Open Magic:

Steps:

1. Draw PMOS and NMOS transistors for NOR topology
2. Create metal/poly routing
3. Label pins: **a**, **b**, **out**, **vdd**, **gnd**
4. Run DRC:

```bash
drc check
```

5. Save layout:

```bash
save nor1_gate.mag
```


## 8. Extract Layout for Spice Simulation

Extract:

```bash
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

This generates:

```
nor1_gate.spice
```

## 9. Transistor-Level Simulation Using Ngspice

Run:

```bash
ngspice nor1_gate.spice
```

Example simulation commands inside Ngspice:

```
plot v(a) v(b) v(out)
```

---

## 10. Final Outputs Produced

* ✔ **RTL file** → `nor1_gate.v`
* ✔ **Testbench** → `tb_nor1_gate.v`
* ✔ **Waveform** → `nor1_gate.vcd`
* ✔ **Synthesized netlist** → `nor1_synth.v`
* ✔ **Layout file** → `nor1_gate.mag`
* ✔ **Extracted SPICE file** → `nor1_gate.spice`
* ✔ **Ngspice waveform plots** `nor1_gate_ngspce_sim`


## 11. Conclusion
 
This project demonstrates the **full chip-design workflow** for a NOR gate, starting from Verilog RTL to physical layout and transistor-level verification.
It shows how open-source EDA tools can be used to perform a complete VLSI design flow in Ubuntu.



## Author

**VENKATESH DAMERA**

**VLSI & EMBEDDED ENTHUSIAST**
