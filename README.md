# 📘 Week 2 Task – BabySoC Fundamentals & Functional Modelling

## 🎯 Objective
To build a solid understanding of **System-on-Chip (SoC) fundamentals** and practice **functional modelling** of the BabySoC using open-source simulation tools like **Icarus Verilog** and **GTKWave**.  

---

## 1. 🔎 What is a System-on-Chip (SoC)?
A **System-on-Chip (SoC)** is an integrated circuit that combines all the essential components of a computer system into a single chip.  
This integration allows for **compact size, low power consumption, cost efficiency, and high performance**.  

---

## 2. ⚡ Components of a Typical SoC
| Component | Description |
|-----------|-------------|
| **CPU (Central Processing Unit)** | Executes instructions, the “brain” of the SoC. |
| **Memory (RAM/ROM/Flash)** | Stores temporary and permanent data. |
| **Peripherals (I/O Devices)** | Interfaces for communication with the outside world (USB, UART, timers, sensors). |
| **Interconnect (Bus/NoC)** | Connects all components together, enabling communication. |

---

## 3. 🧩 Why BabySoC?
The **BabySoC** is a simplified open-source SoC model based on **RISC-V (RVMYTH processor)**.  
It is designed as a **learning platform** that demonstrates key SoC concepts while remaining easy to understand and simulate.  

**Key Features of BabySoC:**
- **RVMYTH CPU** – open-source RISC-V processor core.  
- **Phase-Locked Loop (PLL)** – generates stable and synchronized clock signals.  
- **10-bit DAC (Digital-to-Analog Converter)** – converts digital signals to analog output (e.g., audio/video).  

➡️ BabySoC provides a practical foundation for beginners to **experiment, simulate, and understand SoC behavior** before moving to advanced design stages.  

---

## 4. 🛠️ Role of Functional Modelling
Before proceeding to **RTL design** and **physical implementation**, it is important to verify **functionality** through **simulation models**.

- **Functional Modelling** checks:  
  - Whether the CPU executes instructions correctly.  
  - If the PLL produces stable clock signals.  
  - Whether the DAC correctly converts digital outputs into analog equivalents.  

- **Tools Used:**  
  - **Icarus Verilog** – for simulation of BabySoC design.  
  - **GTKWave** – for waveform visualization of signals and verifying functional correctness.  

This stage ensures that the **logical behavior of the SoC is correct** before moving to RTL synthesis, floorplanning, or layout design.  

---

## ✅ Deliverable – My Understanding
- An **SoC** is a complete computer system on a single chip, integrating CPU, memory, peripherals, and interconnect.  
- **BabySoC** acts as a simplified learning model, helping me understand how digital and analog components interact.  
- Through **functional modelling**, I validated the basic behavior of BabySoC using open-source simulation tools.  
- This exercise strengthens my foundation in **SoC design concepts** and prepares me for upcoming **RTL and physical design** stages.  

---

📌 **Summary:**  
Week 2 focused on building **conceptual understanding** of SoC fundamentals and performing **functional modelling** of BabySoC.  
This provided me with a strong theoretical base and practical simulation experience, bridging the gap between **SoC theory** and **real-world design workflows**.  

---
