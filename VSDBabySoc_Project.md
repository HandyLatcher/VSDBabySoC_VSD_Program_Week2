# VSDBabySoC Overview 💻

**VSDBabySoC** is a compact, educational **System-on-Chip (SoC)** built around a **RISC-V processor (RVMYTH)**. It integrates a **PLL (Phase-Locked Loop)** for stable clock generation and a **10-bit DAC (Digital-to-Analog Converter)** for analog output. The design provides a hands-on platform to learn **SoC design, simulation, and digital-to-analog interfacing**.

---

## Key Components ⚙️

| Module         | Purpose                                                | Inputs                               | Outputs                |
| -------------- | ------------------------------------------------------ | ------------------------------------ | ---------------------- |
| **RVMYTH CPU** | Performs computation and generates 10-bit digital data | `CLK`, `reset`                       | `OUT[9:0]`             |
| **PLL**        | Generates a stable clock signal                        | `VCO_IN`, `ENb_CP`, `ENb_VCO`, `REF` | `CLK`                  |
| **DAC**        | Converts digital signals from CPU to analog voltage    | `D[9:0]`, `VREFH`                    | `OUT` (analog voltage) |

---

## Educational Objectives 🎓

* **Hands-on SoC Integration**: Combine CPU, PLL, and DAC modules.
* **Clock and Signal Management**: Learn how PLLs stabilize clocks.
* **Digital-to-Analog Conversion**: Convert binary outputs to analog signals.
* **Simulation and Verification**: Use **Icarus Verilog** and **GTKWave** for pre- and post-synthesis validation.

---

## Project Structure 📂

```
/home/designer/VSDBabySoC
├── LICENSE
├── Makefile
├── README.md
├── compiled_rvmyth.v
├── compiled_rvmyth_gen.v
├── images
│   ├── centralized_avsddac.png
│   ├── inside_dac.png
│   ├── inside_pll.png
│   ├── openlane_flow.png
│   ├── physical_design.png
│   ├── post_routing_sim.png
│   ├── post_synth_sim.png
│   ├── pre_synth_sim.png
│   ├── rvmyth_layout.png
│   ├── selected_dac.png
│   ├── selected_pll.png
│   ├── vsdbabysoc_block_diagram.png
│   └── vsdbabysoc_layout.png
├── out
├── output
│   ├── compiled_tlv
│   │   └── rvmyth.v
│   ├── post_synth_sim
│   │   ├── post_synth_sim.out
│   │   └── post_synth_sim.vcd
│   ├── pre_synth_sim
│   │   ├── pre_synth_sim.out
│   │   └── pre_synth_sim.vcd
│   └── synth
│       ├── synth.log
│       └── vsdbabysoc.synth.v
└── src
    ├── gds
    │   ├── avsddac.gds
    │   └── avsdpll.gds
    ├── gls_model
    │   ├── primitives.v
    │   └── sky130_fd_sc_hd.v
    ├── include
    │   ├── sandpiper.vh
    │   ├── sandpiper_gen.vh
    │   ├── sp_default.vh
    │   └── sp_verilog.vh
    ├── layout_conf
    │   ├── rvmyth
    │   │   ├── config.tcl
    │   │   └── pin_order.cfg
    │   └── vsdbabysoc
    │       ├── config.tcl
    │       ├── macro.cfg
    │       └── pin_order.cfg
    ├── lef
    │   ├── avsddac.lef
    │   └── avsdpll.lef
    ├── lib
    │   ├── avsddac.lib
    │   ├── avsdpll.lib
    │   └── sky130_fd_sc_hd__tt_025C_1v80.lib
    ├── module
    │   ├── avsddac.v
    │   ├── avsdpll.v
    │   ├── clk_gate.v
    │   ├── pseudo_rand.sv
    │   ├── pseudo_rand_gen.sv
    │   ├── rvmyth.tlv
    │   ├── rvmyth.v
    │   ├── rvmyth_gen.v
    │   ├── testbench.rvmyth.post-routing.v
    │   ├── testbench.v
    │   └── vsdbabysoc.v
    ├── script
    │   ├── sta.conf
    │   ├── verilog_to_lib.pl
    │   └── yosys.ys
    └── sdc
        ├── vsdbabysoc_layout.sdc
        └── vsdbabysoc_synthesis.sdc
```

---

# Pre-Synthesis Simulation of VSDBabySoC 📊

To verify the design functionality before synthesis, we perform a **pre-synthesis simulation** using `iverilog`.
This simulation generates a **VCD (Value Change Dump) file**, which stores the signal transitions and can be visualized using **GTKWave** for analysis.

---

## Compiling the Testbench 🖥️

```bash
iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM -I src/include -I src/module src/module/testbench.v
```

**Explanation:**

* `-DPRE_SYNTH_SIM` → Defines a macro for **conditional compilation** in the testbench.
* `-I src/include -I src/module` → Adds include paths for headers and Verilog modules.
* `-o output/pre_synth_sim/pre_synth_sim.out` → Specifies the output executable location.

---

## Running the Simulation ▶️

```bash
# 1. Navigate to the simulation output directory
cd vsdbabysoc/output/pre_synth_sim

# 2. Run the pre-synthesis simulation executable
./pre_synth_sim.out
```

```
pre_synth_sim.vcd
```

```bash
# 3. Return to the project root directory
cd ../../
```

```bash
# 4. Open the waveform in GTKWave
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

<div align="center">
  <img src="https://github.com/user-attachments/assets/a79f9743-97b7-4c12-b4d0-b92080914cd4" width="800" />
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/a14852f7-2a88-419c-b69e-d413e64f4a3a" width="800" />
  <p><b>Fig: Pre-Synthesis Simulation Waveform</b></p>
</div>


---

## Key Signals 🖲️

* **`CLK`** → Clock signal generated by the PLL, ensuring synchronous operation across all SoC modules.
* **`OUT`** → DAC output, representing digital-to-analog conversion. Changes in the RVMYTH core digital values can be observed in real time.

---

## Interpreting the Results 📝

* **CLK** → Confirms all modules operate **synchronously**.
* **OUT** → Demonstrates **digital-to-analog conversion**, showing data flow from the SoC to the DAC.

This workflow provides **hands-on learning** of digital-to-analog interfacing and RISC-V SoC operation, bridging theory and practical observation.

---

# RTL Synthesis of `vsdbabysoc` using Yosys ⚙️

### Steps

1. **Open Yosys**

   ```bash
   yosys
   ```

2. **Read Verilog RTL source files**

   ```tcl
   read_verilog -sv -I src/include src/module/vsdbabysoc.v
   read_verilog -sv -I src/include src/module/rvmyth.v
   read_verilog -sv -I src/include src/module/clk_gate.v
   ```

3. **Read standard cell libraries**

   ```tcl
   read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_liberty -lib src/lib/avsddac.lib
   read_liberty -lib src/lib/avsdpll.lib
   ```

4. **Run synthesis**

   ```tcl
   synth -top vsdbabysoc
   ```

5. **Technology mapping using ABC**

   ```tcl
   abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

6. **Visualize synthesized design 🖼️**

   ```tcl
   show
   ```

7. **Synthesis for Other Modules**
   The same workflow can be performed for individual modules like `clk_gate` and `rvmyth` to inspect their respective gate-level netlists.

---

<div align="center">
  <img src="https://github.com/user-attachments/assets/c1875965-6dc8-4284-a4d9-708f73d02390" width="800" />
  <p><b>Fig: Synthesized vsdbabysoc netlist</b></p>
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/9a944486-67a7-46aa-aab4-1a8b16c88ee7" width="800" />
  <p><b>Fig: Synthesized clk_gate netlist</b></p>
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/8ec3c9bf-ea76-4152-844e-646668175fcd" width="800" />
  <p><b>Fig: Synthesized rvmyth netlist</b></p>
</div>

---

## Note 💡

The `rvmyth` module is a RISC-V based CPU core. Its synthesized netlist is extremely large and cannot fit on a single screen, highlighting the **scale and complexity** of the processor design.

---

# Post-Synthesis Simulation of VSDBabySoC ⚡

Post-synthesis simulation (Gate-Level Simulation) verifies the functionality of the **synthesized design**.
The simulation generates a **VCD (Value Change Dump) file**, viewable in **GTKWave**.

---

## Compiling the Testbench 🖥️

```bash
iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -I src/include -I src/module src/module/testbench.v
```

**Explanation:**

* `-DPOST_SYNTH_SIM` → Macro for post-synthesis simulation.
* `-I src/include -I src/module` → Include paths for headers and modules.
* `-o output/post_synth_sim/post_synth_sim.out` → Output executable location.

---

## Running the Simulation ▶️

```bash
# 1. Navigate to the post-synthesis simulation directory
cd vsdbabysoc/output/post_synth_sim

# 2. Run the post-synthesis simulation executable
./post_synth_sim.out
```

```
post_synth_sim.vcd
```

```bash
# 3. Return to the project root directory
cd ../../
```

```bash
# 4. Open the waveform in GTKWave
gtkwave output/post_synth_sim/post_synth_sim.vcd
```

<div align="center">
  <img src="https://github.com/user-attachments/assets/d854c32c-2d89-4429-8a9c-429e97e089ef" width="800" />
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/8bec70e5-c7db-4974-9c6c-24741ada09c7" width="800" />
  <p><b>Fig: Post-Synthesis Simulation Waveform</b></p>
</div>

---

## Key Signals 🖲️

* **`CLK`** → Clock signal of the RVMYTH core.
* **`reset`** → Reset signal for initializing the RVMYTH core.
* **`OUT`** → DAC output (simulated digitally in GLS).
* **`\core.OUT[9:0]`** → 10-bit output port of the RVMYTH core.

---

# Project Summary 📌

**VSDBabySoC** is an educational RISC-V based SoC integrating a CPU (RVMYTH), PLL, and 10-bit DAC. The project covers the complete workflow: **pre-synthesis simulation** to verify functionality, **RTL synthesis** with Yosys, and **post-synthesis simulation** to validate the synthesized design. Key signals can be observed in GTKWave, allowing hands-on experience in **SoC design, digital-to-analog interfacing, and verification**.

---
