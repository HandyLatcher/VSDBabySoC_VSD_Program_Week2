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
VSDBabySoC/
├── src/
│   ├── include/        # Header files with parameters/macros (*.vh)
│   ├── module/         # Verilog source files
│   │   ├── vsdbabysoc.v    # Top-level SoC
│   │   ├── rvmyth.v        # CPU core
│   │   ├── avsdpll.v       # PLL module
│   │   ├── avsddac.v       # DAC module
│   │   └── testbench.v     # Testbench for simulations
└── output/             # Compiled outputs and simulation results

```

---

## Simulation Workflow ⚡

### Pre-Synthesis Simulation

```bash
iverilog -o output/pre_synth_sim/out \
    -DPRE_SYNTH_SIM -I src/include -I src/module \
    src/module/testbench.v src/module/vsdbabysoc.v
cd output/pre_synth_sim
./out
gtkwave pre_synth_sim.vcd
```


* Defines `PRE_SYNTH_SIM` macro for conditional compilation.
* Generates a `.vcd` waveform to visualize module behavior.

### Post-Synthesis Simulation

```bash
iverilog -o output/post_synth_sim/out \
    -DPOST_SYNTH_SIM -I src/include -I src/module \
    src/module/testbench.v output/synthesized/vsdbabysoc.synth.v
cd output/post_synth_sim
./out
gtkwave post_synth_sim.vcd
```

* Uses synthesized Verilog for timing-accurate simulation.

---

## Troubleshooting Tips 🛠️

* **Multiple Definitions**: Include each module only once in simulation commands.
* **Path Errors**: Use absolute paths if relative paths cause `file not found` issues.
* **GTKWave Issues**: Ensure `.vcd` files are properly generated before opening.

---

## Summary ✅

VSDBabySoC is a **practical learning platform** for students and engineers to explore SoC design, RISC-V cores, clock management via PLL, and digital-to-analog conversion. By combining **simulation, synthesis, and verification**, it bridges the gap between theory and hands-on hardware experience.

---


