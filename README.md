# ⚡VSDBabySoC

**VSDBabySoC** is a compact, educational **System-on-Chip (SoC)** built on the **RISC-V** architecture.  
It integrates an **RVMYTH CPU**, a **Phase-Locked Loop (PLL)** for stable clock generation ⏱️, and a **10-bit Digital-to-Analog Converter (DAC)** 🎛️ to interact with analog devices.  
This project is perfect for learning digital-analog interfacing and modern SoC design.

---

## 🎯 Problem Statement

VSDBabySoC aims to:

- Combine a **RISC-V CPU** with analog components (PLL + DAC).  
- Output to devices that accept analog signals, like **TVs 📺** and **mobile phones 📱**.  
- Provide a **well-documented, educational platform** for experimentation.  
- Be implemented on **Sky130 technology**.

---

## 💡 What is a System on Chip (SoC)?

A **System-on-Chip (SoC)** puts all the essential parts of a computer on a single chip 💻.  
This makes devices smaller, faster, and more energy-efficient 🔋.

**Key components of an SoC:**

| Component | Function |
|-----------|---------|
| CPU  | Processes instructions and data. |
| Memory  | RAM for temporary storage, Flash/ROM for permanent storage. |
| I/O Ports  | Connects the SoC to peripherals like cameras, USBs, or headphones. |
| GPU  | Renders graphics and video. |
| DSP  | Optimizes audio/video signal processing. |
| Power Management  | Efficiently manages energy usage. |
| Special Features  | Wi-Fi, Bluetooth, or security modules depending on the design. |

**Why SoCs rock 🤘:**

- Space-saving 📏  
- Energy-efficient ⚡  
- Fast 🚀  
- Cost-effective 💰  
- Reliable ✅  

---

## 🧩 Types of SoCs

1. **Microcontroller-based**: Low-power, simple control tasks .  
2. **Microprocessor-based**: Handles demanding apps, runs OS .  
3. **Application-Specific (ASIC)**: Optimized for specialized tasks like graphics, AI, or multimedia .

---

## VSDBabySoC Components

| Component | Description |
|-----------|------------|
| **RVMYTH CPU**  | Processes data and communicates with DAC. |
| **PLL ** | Generates a stable clock for the CPU and DAC. |
| **DAC ** | Converts digital signals to analog output for external devices. |

**How it works:**

1. **Initialization & Clock Generation**: PLL stabilizes the clock.  
2. **Data Processing**: RVMYTH updates its registers with new data.  
3. **Analog Output**: DAC converts digital data into analog signals for devices.

---

## ⏱️ About PLL

A **Phase-Locked Loop (PLL)** synchronizes output signals with a reference input.

**Main parts:**

- **Phase Detector**: Compares input & output phases.  
- **Loop Filter**: Smooths error signals.  
- **Voltage-Controlled Oscillator (VCO)**: Adjusts frequency to match input.  

**Why not use off-chip clocks?**

- On-chip clocks reduce **delays** and **jitter** ⚡  
- Supports **multiple frequencies** for different blocks 🧩  
- More **stable and precise timing** ✅

---

## 🎛️ About DAC

A **Digital-to-Analog Converter (DAC)** turns digital binary input into analog output.

**Types of DACs:**

- **Weighted Resistor DAC** 
- **R-2R Ladder DAC**  

VSDBabySoC uses a **10-bit DAC** to interface with real-world analog devices.

---

## ✅ Summary

VSDBabySoC is a hands-on **learning SoC**, showing how a RISC-V CPU, PLL, and DAC work together.  
By studying this project, you’ll get a **solid foundation in embedded systems design** and **digital-to-analog interfacing** 🧩🎉.
