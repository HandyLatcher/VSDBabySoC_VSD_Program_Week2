# 📘 Week 2 – BabySoC Fundamentals & Functional Modelling

## 🎯 Objective
Understand **SoC fundamentals** and practice **functional modelling** of BabySoC using **Icarus Verilog** and **GTKWave**.

---

## 1. What is a System-on-Chip (SoC)?
A **System-on-Chip (SoC)** integrates all essential components of a computer system into a **single chip**, including CPU, memory, peripherals, and interconnects.  
This allows for **compact size, low power consumption, cost efficiency, and high performance**, making SoCs ideal for smartphones, wearables, tablets, and embedded systems.

---

## 2. Key Components of a Typical SoC

| Component | Description |
|-----------|-------------|
| **CPU** | Executes instructions, the “brain” of the SoC. |
| **Memory** | RAM (temporary) and ROM/Flash (permanent) for data storage. |
| **I/O Ports** | Interfaces for communication with external devices. |
| **GPU** | Renders visuals, animations, and video. |
| **DSP** | Specialized processor for audio, video, and signal processing. |
| **PMU** | Manages power consumption and efficiency. |
| **Clock & Timing** | Often uses PLLs to synchronize system operations. |
| **Analog Components / DAC** | Converts digital outputs to analog signals for external devices. |

---

## 3. Types of SoCs

| Type | Features | Example Applications |
|------|----------|--------------------|
| Microcontroller-based | Simple, low-power, minimal processing | IoT devices, home appliances |
| Microprocessor-based | Supports OS, multitasking, higher processing | Smartphones, tablets |
| Application-Specific (ASIC) | Custom-designed for high performance | AI accelerators, graphics cards |

<p align="center">
  <img src="https://github.com/user-attachments/assets/efaf2c1c-7c47-4d5c-af64-65e64a5e12eb" width="800" alt="SoC Block Diagram" />
</p>

---

## 4. Introduction to VSDBabySoC
**VSDBabySoC** is a compact RISC-V-based SoC designed for **learning and experimentation**.  
It facilitates **simultaneous testing of three open-source IP cores** and calibration of analog components.

**Key Features:**
- **RVMYTH CPU** – Processes instructions and drives DAC inputs.  
- **PLL** – Generates a stable, synchronized clock for CPU and DAC.  
- **10-bit DAC** – Converts digital signals from CPU into analog output for external devices.  

<p align="center">
  <img src="https://github.com/user-attachments/assets/2935b698-8ed4-439b-88f4-d62c180eea72" width="800" alt="BabySoC Architecture" />
</p>

---

## 5. BabySoC Functional Flow

| Stage | Function |
|-------|---------|
| **Initialization & Clocking** | PLL locks to input reference and generates a stable clock. |
| **Data Processing (RVMYTH CPU)** | Updates r17 register sequentially with values for DAC conversion. |
| **Analog Output (DAC)** | Converts digital values into analog signals, saved in `OUT`, for devices like TVs or speakers. |

---

## 6. Phase-Locked Loop (PLL)
A **PLL** synchronizes output frequency with an input reference.  

**Components:**
| Component | Role |
|-----------|------|
| Phase Detector | Compares input and feedback signals; outputs error voltage. |
| Loop Filter | Smooths error voltage to produce control voltage. |
| VCO (Voltage-Controlled Oscillator) | Adjusts frequency based on control voltage to match reference. |

**Function:** Locks output frequency and phase to the input; may use feedback dividers to multiply frequency.

<p align="center">
  <img src="https://github.com/user-attachments/assets/e5bf9c2e-a55b-499b-b0ee-1854b4c7913b" width="800" alt="BabySoC Functional Flow" />
</p>


**Why Off-Chip Clocks Are Limited:**  
- Distribution delays, jitter, and varying block frequency requirements.  
- Crystal frequency deviation, stability issues, and aging can reduce timing accuracy.

---

## 7. Digital-to-Analog Converter (DAC)🎚️
Converts digital input into an analog voltage.  

## Types

## Weighted Resistor DAC:
Uses resistors with bit-dependent weights.

<p align="center">
  <img src="https://github.com/user-attachments/assets/cbd7db5f-83fb-4912-b8d9-fefb041de4e3" width="800" alt="BabySoC DAC Illustration" />
</p>

## R-2R Ladder DAC:
Uses a repeating resistor network for simple scaling.

<p align="center">
  <img src="https://github.com/user-attachments/assets/812a2f15-1342-4f55-8ba4-4b3c809bde11" width="800" alt="R-2R Ladder DAC" />
</p>

**In VSDBabySoC:**  
- 10-bit DAC converts CPU digital outputs to analog signals.  
- Supports digital-analog interfacing for multimedia output.

---

## 8. Role of Functional Modelling🛠️
Functional modelling **verifies logic** before RTL and physical design:

- Validates CPU instructions, PLL timing, and DAC outputs.  
- Detects functional errors early in simulation.  
- **Tools Used:**  
  - **Icarus Verilog** → Compiles and simulates BabySoC RTL.  
  - **GTKWave** → Visualizes waveforms for signal and timing analysis.

---

## ✅ Key Learnings
- SoCs integrate CPU, memory, peripherals, and analog/digital blocks into **one chip**.  
- BabySoC demonstrates **digital-analog interfacing** and educational exploration of RISC-V.  
- PLL ensures **stable, synchronized timing** for CPU and DAC.  
- DAC enables **real-world analog output** from digital processing.  
- Functional modelling validates system behavior **before synthesis and physical implementation**.

---

📌 **Summary:**  
Week 2 focused on **SoC fundamentals, BabySoC architecture, and functional modelling**, providing a foundation in **digital-analog interfacing, clock management, and SoC design**, preparing for RTL design and SoC tapeout exercises.

---
