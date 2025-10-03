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

## Step-by-Step VSDBabySoC Modeling Walkthrough ⚡

This section demonstrates **how to simulate and observe the VSDBabySoC behavior**. We will modify digital values, feed them to the DAC, and monitor the analog output. These instructions are **tested on Ubuntu 18.04.5 (Bionic)**.

---

### 1. Install Required Tools

First, install all necessary packages:

```bash
sudo apt update
sudo apt install make python3 python3-pip git iverilog gtkwave docker.io
sudo chmod 666 /var/run/docker.sock
```

Next, install Python packages:

```bash
pip3 install pyyaml click sandpiper-saas
```

These tools allow you to **compile, simulate, and visualize waveforms**.

---

### 2. Clone the Project Repository

Choose a working directory (e.g., home directory) and clone the repository:

```bash
cd ~
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC
```

You now have all source files and scripts needed for simulation.

---

## Quick Tip💡

The `rvmyth.v` and `rvmyth_gen.v` files are initially **generated in the `output/` directory**. Once generated, they can be easily moved to the source modules folder using the `mv` command:

```bash
mv output/rvmyth.v src/module/
mv output/rvmyth_gen.v src/module/
```
This keeps the workflow clean and organized for integration into the VSDBabySoC project.

---


# Pre-Synthesis Simulation of VSDBabySoC 📊

To verify the design functionality before synthesis, we perform a **pre-synthesis simulation** using `iverilog`.
This simulation generates a **VCD (Value Change Dump) file**, which stores the signal transitions and can be visualized using **GTKWave** for analysis.

---

## Compiling the Testbench

To compile the VSDBabySoC **before synthesis**, use the following command:

```bash
iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM -I src/include -I src/module src/module/testbench.v
```

**Explanation:**

* `-DPRE_SYNTH_SIM` → Defines a macro for **conditional compilation** in the testbench.
* `-I src/include -I src/module` → Adds include paths for headers and Verilog modules.
* `-o output/pre_synth_sim/pre_synth_sim.out` → Specifies the output executable location.

---

## Running the Simulation

### Steps

```bash
# 1. Navigate to the simulation output directory
cd vsdbabysoc/output/pre_synth_sim

# 2. Run the pre-synthesis simulation executable
./pre_synth_sim.out
```

After execution, the simulation generates the waveform file:

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
  <p><b>Fig: Pre-Synthesis Simulation Waveform</b></p>
</div>

---

## Key Signals

Focus on the **two primary signals**:

* **`CLK`** → Clock signal generated by the PLL, ensuring synchronous operation across all SoC modules.
* **`OUT`** → DAC output, representing digital-to-analog conversion. Changes in the RVMYTH core digital values can be observed in real time.

---

## Interpreting the Results 📝

* **CLK** → Confirms all modules operate **synchronously**.
* **OUT** → Demonstrates **digital-to-analog conversion**, showing data flow from the SoC to the DAC.

This workflow provides **hands-on learning** of digital-to-analog interfacing and RISC-V SoC operation, bridging theory and practical observation.

---



## RTL Synthesis of `vsdbabysoc` using Yosys⚙️

### Steps

1. **Open Yosys**

   ```bash
   yosys
   ```

2. **Read Verilog RTL source files**
   These files contain the design description of the SoC.

   ```tcl
   # Read Verilog RTL files
   read_verilog -sv -I src/include src/module/vsdbabysoc.v
   read_verilog -sv -I src/include src/module/rvmyth.v
   read_verilog -sv -I src/include src/module/clk_gate.v
   ```

3. **Read standard cell libraries**
   These libraries define the characteristics of the standard cells used for synthesis.

   ```tcl
   # Read standard cell libraries
   read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_liberty -lib src/lib/avsddac.lib
   read_liberty -lib src/lib/avsdpll.lib
   ```

4. **Run synthesis**
   Specify the **top module** of the design (`vsdbabysoc`).

   ```tcl
   synth -top vsdbabysoc
   ```

5. **Technology mapping using ABC**
   Map the synthesized design to standard cells defined in the liberty file.

   ```tcl
   abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

6. **Visualize synthesized design**
   Yosys can display a graphical view of the synthesized circuit.

   ```tcl
   show
   ```
7. **Synthesis for Other Modules**
The same synthesis and visualization workflow applied to `vsdbabysoc` can also be performed for individual modules such as `clk_gate` and `rvmyth`.  
By specifying the desired module as the top module in Yosys, you can generate and inspect their respective gate-level netlists, allowing detailed analysis of each component of the design.


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

## Note:💡
The rvmyth module is a RISC-V based CPU core. Consequently, the synthesized netlist is extremely large and complex, containing numerous interconnected logic elements. Due to its size, its difficult to display the netlist on a single screen, which highlights the scale and intricacy of this processor design.

---

Absolutely! I’ve rewritten your **Post-Synthesis Simulation (Gate-Level Simulation)** section to match the style, formatting, and clarity of your **Pre-Synthesis Simulation** section, making it GitHub-ready, concise, and professional.

---


## Post-Synthesis Simulation of VSDBabySoC ⚡

Post-synthesis simulation, also known as **Gate-Level Simulation (GLS)**, verifies the functionality of the **synthesized design** including the mapped standard cells.
The simulation generates a **VCD (Value Change Dump) file**, which can be visualized using **GTKWave**.

---

## Compiling the Testbench

To compile the VSDBabySoC **after synthesis**, use the following command:

```bash
iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -I src/include -I src/module src/module/testbench.v
```

**Explanation:**

* `-DPOST_SYNTH_SIM` → Defines a macro for **post-synthesis simulation**.
* `-I src/include -I src/module` → Adds include paths for headers and Verilog modules.
* `-o output/post_synth_sim/post_synth_sim.out` → Specifies the output executable location.

---

## Running the Simulation

### Steps

```bash
# 1. Navigate to the post-synthesis simulation directory
cd vsdbabysoc/output/post_synth_sim

# 2. Run the post-synthesis simulation executable
./post_synth_sim.out
```

After execution, the simulation generates the waveform file:

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
  <p><b>Fig: Post-Synthesis Simulation Key Signals</b></p>
</div>


## Key Signals

Focus on the **primary signals**:

* **`CLK`** → Clock signal of the RVMYTH core, ensuring synchronous operation.
* **`reset`** → Reset signal for initializing the RVMYTH core.
* **`OUT`** → DAC output (simulated as digital due to GLS limitations).
* **`\core.OUT[9:0]`** → 10-bit output port of the RVMYTH core.

---























