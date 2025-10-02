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

Here’s a polished, professional version of your section:

---

### 3. Run Pre-Synthesis Simulation ⚡

To compile and simulate the SoC **before synthesis**, execute:

```bash
make pre_synth_sim
```

* This generates a **waveform file** in VCD format:

```
output/pre_synth_sim/pre_synth_sim.vcd
```

* The simulation allows you to verify module functionality and signal behavior prior to synthesis.

---

Here’s a fully **polished, professional version** of that section with the **corrected `iverilog` command**, explanation, and GTKWave instructions integrated smoothly:

---

### 3. Run Pre-Synthesis Simulation ⚡

To compile and simulate the VSDBabySoC **before synthesis**, use the following command:

```bash
iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM \
    -I src/include -I src/module \
    src/module/testbench.v
```

**Explanation:**

* The `-DPRE_SYNTH_SIM` flag defines a macro for **conditional compilation** in the testbench.
* `-I src/include -I src/module` adds the include paths for headers and Verilog modules.


Running this command generates the **pre-synthesis waveform**:

```
output/pre_synth_sim/pre_synth_sim.vcd
```

---

### 4. Visualize Waveforms 📊

Open the waveform in GTKWave:

```bash
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

Focus on the **two key signals**:

* `CLK` → Clock signal generated by the PLL.
* `OUT` → DAC output, reflecting the conversion of digital signals to analog voltage.

By modifying the digital values in the RVMYTH core, you can **observe the DAC output change in real time**.

---

### 5. Interpreting the Results 📝

* **CLK** ensures that all SoC modules operate **synchronously**.
* **OUT** demonstrates the **digital-to-analog conversion**, showing how data flows through the system.

This workflow provides **hands-on learning** of digital-to-analog interfacing and RISC-V SoC operation, bridging theory and practical observation.

---




