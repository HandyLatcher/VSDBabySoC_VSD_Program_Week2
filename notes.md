# 🗒️Notes

This section highlights key terms and concepts encountered during the study of BabySoC functional modelling. It serves as a quick reference to understand important technical details and clarify new terminology used throughout the project.

---

## 1. CPU vs CPU Core🖥️

- **CPU (Central Processing Unit)**: Entire processor chip, acts as the brain of the computer.  
- **CPU Core**: Individual processing unit inside the CPU that executes instructions.  
➡️ A CPU may contain multiple cores (dual-core, quad-core, etc.) for parallel processing.


---

## 2. Working of a Phase-Locked Loop (PLL)🔁

**Step 1: Initial Condition**  
- Input frequency = 100 Hz  
- VCO free-running = 300 Hz  
- VCO gain = 50 Hz/V  
- Feedback divider = 4 → VCO/4 = 75 Hz  

**Step 2: Phase Detector**  
- Compares input (100 Hz) with divided VCO (75 Hz).  
- Outputs error signal (positive since input > feedback).  

**Step 3: Loop Filter**  
- Smooths PD output → produces control voltage (say 0.5 V).  

**Step 4: VCO Adjustment**  
- VCO = 300 + 50×0.5 = 325 Hz  
- Divided feedback ≈ 81.25 Hz → still < input.  
- Error keeps adjusting until lock.  

**Step 5: Locking**  
- Condition met when VCO/4 = 100 Hz → VCO = 400 Hz.  
- 🔒 PLL Locked.  

**Step 6: Output**  
- Input = 100 Hz  
- Output = 400 Hz (multiplied by N=4).  

---

## 3. Weighted Resistor DAC🎚️

**Example: 3-bit, Vref=20 V**  
- Input = `101` → Decimal = 5  
- Step size = 20 / (2³–1) = 2.857 V  
- Output = 5 × 2.857 ≈ **14.3 V** ✅  

**Resistor Mapping**  
| Bit | Weight | Resistor |
|-----|--------|----------|
| MSB (b₂) | 4 | 10 kΩ |
| Middle (b₁) | 2 | 20 kΩ |
| LSB (b₀) | 1 | 40 kΩ |

**Current Calculation**  
- MSB: 2 mA  
- Middle: 0 mA  
- LSB: 0.5 mA  
- Total = 2.5 mA  

**Feedback Resistor**  
- Rf = Vout / Itotal = 14.285 / 0.0025 ≈ **5.7 kΩ**.  

**Full-Scale Check (111)**  
- Currents = 2 + 1 + 0.5 = 3.5 mA  
- Output = 20 V ✅

---

## 4. R-2R Ladder DAC🔀

- Uses resistor ladder with MSB at rightmost branch.  
- Thevenin’s theorem used to calculate equivalent Vout.  
- Advantage: only two resistor values (R, 2R).

---


