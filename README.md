# Ultra-Low-Noise-Ku-Band-LNA-in-TSMC-65nm

Cadence Virtuoso implementation of an ultra-low noise Ku-Band (10.7–12.75 GHz) LNA in TSMC 65nm bulk CMOS, utilizing Simultaneous Noise and Impedance Matching (SNIM) to achieve a 1.06 dB Noise Figure.

## Project Overview
This repository contains the full-custom transistor-level design and simulation documentation for an ultra-low noise Ku-Band LNA. Developed as part of the EE698L RFIC Design coursework at IIT Kanpur, the LNA is implemented using the TSMC 65nm bulk CMOS process. 

The primary objective of this project was to match and exceed the performance of a commercial Silicon-on-Insulator (SOI) benchmark RFIC (the Renesas F6931) using a standard, lossy bulk CMOS substrate.

## Design Specifications (vs. Commercial Target)
The circuit was aggressively optimized to meet the following parameters:
* **Frequency Band**: 10.7 GHz to 12.75 GHz (Ku-Band), centered at 11.725 GHz.
* **Noise Figure (NF)**: 1.06 dB achieved (Surpassing the 1.1 dB commercial SOI target).
* **Forward Gain (S21)**: 30.32 dB achieved (Exceeding the 24 dB target).
* **Linearity**: 14.43 dBm OIP3 and 2.43 dBm OP1dB (Exceeding 8 dBm and -1 dBm targets).
* **Input/Output Match**: S11 = -22.62 dB, S22 = -13.45 dB.
* **Stability**: Unconditionally stable across the entire band ($K_f = 4.78$).

---

## System Architecture

To break the noise floor limitations of bulk CMOS, this design discards traditional LNA topologies in favor of a specialized approach:

### 1. The SNIM Strategy (Eliminating $L_G$)
Traditional LNAs rely on a noisy series gate inductor ($L_G$) for 50-ohm matching. On a lossy bulk substrate, the parasitic resistance of $L_G$ degrades the noise figure. This design eliminates $L_G$ entirely. Instead, it relies on Simultaneous Noise and Impedance Matching (SNIM)—using massive intrinsic device scaling to directly align the optimum noise impedance ($Z_{opt}^*$) and input impedance onto the constant admittance circle.

### 2. Stage 1: Core SNIM & ESD Protection
* **High-Parallelism Layout**: Utilizes a $240 \mu m$ NMOS device highly parallelized ($12 \times 20$) to push the physical gate resistance ($R_g$) down to $\sim 1 \Omega$.
* **Susceptance Cancellation**: A dual-function shunt inductor ($L_{ESD} \approx 657 pH$) provides the exact negative susceptance required to cancel the massive $C_{gs}$, achieving a deep 50-ohm match. Crucially, as a grounded element, it acts as a robust DC discharge path, protecting the thin 65nm gate oxide from ESD events without adding series thermal noise.

### 3. Stage 2: Cascode Linearity & Gain Flattening
* **Linearization**: The second stage is scaled down to $192 \mu m$ to conserve power and employs inductive source degeneration ($L_S \approx 72 pH$). This creates series-series negative AC feedback, massively boosting the Output IP3 to 14.43 dBm.
* **Wideband Stability**: A $300 \Omega$ parallel drain resistor dampens the LC resonance at the output, flattening the gain across the entire 2 GHz Ku-band bandwidth.

---

## 📘 Detailed Mathematical Calculations & Sizing
> For in-depth theoretical formulations, Smith chart locus analysis, SNIM component extraction equations, and power-vs-noise trade-off calculations, **please refer to the attached Project Presentation PDF (`EE698L_Project_PPT.pdf`) included in this repository.**

---

## Simulation Results (Virtuoso ADE_Explorer)
The LNA was rigorously simulated to ensure robust performance across varying **Voltage** (1.1V, 1.2V, 1.3V) and **Temperature** (-40°C, 25°C, 85°C) corners. *(Note: Process corners were excluded from this specific simulation set to isolate voltage/temperature dependence)*. By operating the SNIM topology at a higher current density (1.2V, 51.44 mW), the design successfully overcomes the inherent substrate losses of bulk CMOS, matching the noise profile of advanced SOI technologies.

* **Noise Figure**: Peaks at an ultra-low 1.06 dB at 11.725 GHz, remaining highly competitive even at the worst-case 85°C temperature corner.
* **Isolation**: The cascode topology guarantees excellent reverse isolation ($S12 < -50 dB$), ensuring unconditional stability.
* **Power Handling**: Achieves an Output P1dB of 2.43 dBm.

---

## Result Imagery

* **Complete LNA Schematic:**
<img width="1482" height="730" alt="schematic_Ku-LNA" src="https://github.com/user-attachments/assets/65985773-de77-44c0-aae3-93e4f9f2d629" />
&nbsp;

* **Noise Figure (NF) Across Voltage & Temperature:**
<img width="1614" height="532" alt="image" src="https://github.com/user-attachments/assets/70d2fa66-a435-46bd-8678-21477d4e2c67" />
&nbsp;

* **Forward Gain (S21) Across Voltage & Temperature:**
<img width="1630" height="540" alt="image" src="https://github.com/user-attachments/assets/0031304a-d228-4ba5-bf8b-b180f5807ce6" />
&nbsp;

* **Input Return Loss (S11):**
<img width="1620" height="537" alt="image" src="https://github.com/user-attachments/assets/3467d84a-352b-4de1-b3cc-4e0bc9ef2c37" />
&nbsp;

* **Output Return Loss (S22):**
<img width="1615" height="534" alt="image" src="https://github.com/user-attachments/assets/9f2a33cc-74a8-4b31-98aa-6025b0eb8b62" />
&nbsp;

* **Reverse Isolation (S12):**
<img width="1615" height="534" alt="image" src="https://github.com/user-attachments/assets/1da7519b-29d1-4e22-97fc-e3273cce9aaf" />
&nbsp;

* **Stability Factors (Kf & B1f):**
<img width="1910" height="825" alt="Kf_B1f_Ku-LNA" src="https://github.com/user-attachments/assets/ae9ee100-c466-4290-8714-400b32788990" />
&nbsp;

* **Linearity (Output IP3):**
<img width="1910" height="825" alt="output_IP3_Ku-LNA" src="https://github.com/user-attachments/assets/bbe7b7a4-d135-4060-b809-fbd20b6b8636" />
&nbsp;

* **Power Handling (OP1dB):**
<img width="1910" height="825" alt="output_P1db_Ku-LNA" src="https://github.com/user-attachments/assets/7c2a5670-a6ce-46e6-aba6-f60352127239" />
&nbsp;

