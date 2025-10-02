# VSDBabySoC

**VSDBabySoC** is a small mixed-signal SoC including a **PLL**, **DAC**, and a **RISCV-based processor** named **RVMYTH**. The primary purpose of this SoC is to integrate open-source IP cores, test their interoperability, and calibrate the analog parts.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Problem Statement](#problem-statement)
3. [SoC Components](#soc-components)
    - [What is SoC](#what-is-soc)
    - [What is RVMYTH](#what-is-rvmyth)
    - [What is PLL](#what-is-pll)
    - [What is DAC](#what-is-dac)
4. [VSDBabySoC Modeling](#vsdbabysoc-modeling)
    - [RVMYTH Modeling](#rvmyth-modeling)
    - [PLL and DAC Modeling](#pll-and-dac-modeling)
    - [Step by Step Modeling Walkthrough](#step-by-step-modeling-walkthrough)
5. [OpenLANE Setup](#openlane-setup)
6. [Post-Synthesis Simulation](#post-synthesis-simulation)
7. [Static Timing Analysis (STA)](#static-timing-analysis-sta)
8. [VSDBabySoC Physical Design](#vsdbabysoc-physical-design)
9. [Mixed-Signal RTL2GDSII Flow](#mixed-signal-rtl2gdsii-flow)
10. [Future Work](#future-work)
11. [Contributors & Acknowledgements](#contributors--acknowledgements)

---

## Introduction

VSDBabySoC is a small yet powerful SoC designed to:

- Test integration of open-source IP cores.
- Calibrate analog components.
- Serve educational purposes for digital & analog IC design.

It contains:

| Component | Description |
|-----------|-------------|
| **RVMYTH** | RISCV-based microprocessor |
| **PLL**    | 8x Phase-Locked Loop for stable clock |
| **DAC**    | 10-bit Digital-to-Analog Converter for analog interface |

---

## Problem Statement

- Integrate **RVMYTH CPU**, **PLL**, and **DAC** into a single SoC.
- Allow analog devices (like TVs and mobile phones) to interact with DAC output.
- Fabricate using **Sky130 technology**.
- Serve as a fully open-source, well-documented SoC for learning purposes.

---

## SoC Components

### What is SoC
A **System-on-Chip (SoC)** integrates multiple IP cores on a single die, ranging from microprocessors to analog devices like 5G modems.

### What is RVMYTH
- Simple **RISCV-based CPU**.
- Developed in a **5-day workshop** by RedwoodEDA & VSD.
- Written in **TL-Verilog**, compiled via **Sandpiper-SaaS**.

### What is PLL
- **Phase-Locked Loop**: synchronizes output signal phase with input.
- Used for clock generation & distribution.

### What is DAC
- **Digital-to-Analog Converter**: converts digital signals to analog.
- Used in modern communication and multimedia systems.

---

## VSDBabySoC Modeling

### RVMYTH Modeling
- Designed using **TL-Verilog**.
- Compiled to Verilog using **Sandpiper-SaaS**.
- Reference repo: [RVMYTH](https://github.com/redwoodeda/rvmyth)

### PLL and DAC Modeling
- Verilog cannot synthesize analog designs directly.
- Simulation uses `real` datatype for analog behavior.
- Reference repos:
  - [PLL](https://github.com/lakshmi-sathi/avsdpll_1v8)
  - [DAC](https://github.com/vsdip/avsddac_3v3_sky130_v1)

---

### Step by Step Modeling Walkthrough

1. Install dependencies:

```bash
sudo apt install make python3 python3-pip git iverilog gtkwave docker.io
sudo chmod 666 /var/run/docker.sock
pip3 install pyyaml click sandpiper-saas
````

2. Clone repo:

```bash
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC
```

3. Run pre-synthesis simulation:

```bash
make pre_synth_sim
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

**Important Signals**:

| Signal         | Description                            |
| -------------- | -------------------------------------- |
| CLK            | Clock input from PLL                   |
| reset          | Reset input to RVMYTH                  |
| OUT            | DAC output (analog simulation)         |
| RV_TO_DAC[9:0] | 10-bit output from RVMYTH register #17 |

---

## OpenLANE Setup

* RTL-to-GDSII flow automated using **OpenLANE**.
* Installation:

```bash
git clone https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane/
make openlane
make pdk
make test
```

* Tested commit: `8580c248a995b575f7734813b80bb6c4aa82d4f2`

---

## Post-Synthesis Simulation

* Synthesized using **Yosys**.
* Gate-Level Simulation (GLS) performed using **Iverilog**.
* Outputs verified against pre-synthesis simulation.
* Resulting file: `output/post_synth_sim/post_synth_sim.vcd`

---

## Static Timing Analysis (STA)

* STA performed using **OpenSTA**.
* Clock: `PLL.CLK`
* Sample SDC:

```tcl
set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period 11
```

* Slack for design: **MET** (timing met).

---

## VSDBabySoC Physical Design

* Tools: **OpenLANE, Magic, OpenROAD**
* Steps:

  1. Floorplanning
  2. Placement
  3. Routing
  4. Post-routing STA & simulation
* Final GDSII layouts available in:

```text
output/vsdbabysoc_layout/vsdbabysoc_test/results/magic/vsdbabysoc.gds
```

---

## Mixed-Signal RTL2GDSII Flow

* Analog IPs (PLL, DAC) instantiated as black-boxes.
* Files required per analog IP:

  * **LIB**: timing & power info
  * **GDS**: layout geometry
  * **LEF**: abstract cell info for PnR
* Scripts automate LIB generation from Verilog models.

---

## Future Work

* Fix **LVS mismatches**.
* Resolve ~635 **DRC errors**.
* Fabrication-ready design in Sky130 process.

---

## Contributors & Acknowledgements

* **Mohammad A. Nili** - MS Student, SRBIAU
* **Steve Hoover** - Founder, Redwood EDA
* **Kunal Ghosh** - Co-founder, VSD Corp.
* **Shivani Shah** - Research Scholar, IIIT Bangalore

---

## License

This project is licensed under the **Apache-2.0 License**.

---

Do you want me to do that next?
```
