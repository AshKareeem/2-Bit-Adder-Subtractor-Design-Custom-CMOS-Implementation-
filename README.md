# ⚡ 2-Bit Adder/Subtractor Design (Custom CMOS Implementation)

![Status](https://img.shields.io/badge/Status-Completed-success)
![Tools](https://img.shields.io/badge/Tools-Synopsys%20Custom%20Compiler%20|%20HSPICE-blue)
![Tech](https://img.shields.io/badge/Technology-28nm%20CMOS%20(1.0V)-orange)

A full-custom transistor-level implementation of a 2-bit Adder/Subtractor circuit. This project follows a rigorous **Bottom-Up Design Flow**, starting from manually sized MOSFETs to build logic gates, which were then integrated into a full arithmetic system.

---

## 📌 Project Overview
The goal of this project was to understand the physical and electrical properties of digital circuits by building them from scratch without Standard Cell Libraries.

**Key Technical Specs:**
* **Technology Node:** 28nm PDK (Synopsys).
* **Supply Voltage:** 1.0V.
* **Logic Style:** Static CMOS (Complementary MOS).
* **Transistor Sizing:** Ratio driven ($W_p \approx 2 \times W_n$) to compensate for lower hole mobility and achieve balanced Rise/Fall times.

---

## 🛠️ Design Workflow

The project was executed in three distinct stages, with verification checkpoints at every step:

### Phase 1: Custom Gate Design
* **NAND Gate:** Constructed using 4 transistors (Series NMOS, Parallel PMOS).
    * *Sizing:* NMOS (PDN) = 0.2µm, PMOS (PUN) = 0.2µm (optimized for 1-transistor PMOS path vs 2-transistor NMOS path).
* **XOR Gate:** Constructed using 12 transistors for optimized layout.
    * *Sizing:* NMOS (PDN) = 0.2µm, PMOS (PUN) = 0.4µm (double width to balance mobility).
* **Verification:** Each gate was tested by simulating a full Truth Table.

### Phase 2: 1-Bit Full Adder Integration
* The verified NAND and XOR gates were wired together to form the logic for Sum ($A \oplus B \oplus C_{in}$) and Carry Out.
* **Verification:** Verified against the standard Full Adder truth table using the same period-doubling stimulus method ($T, 2T, 4T$).

### Phase 3: 2-Bit System (Adder/Subtractor)
* **Architecture:** Two 1-bit Full Adders were cascaded (Ripple Carry).
* **Subtraction Logic:** XOR gates were added to the `B` inputs, controlled by a Mode bit ($M$).
    * If $M=0$: $B$ passes unchanged (Addition).
    * If $M=1$: $B$ is inverted, and $C_{in}$ is set to 1 (2's Complement Subtraction).

---

## ⚙️ Circuit Schematics & Logic

### 1. Transistor Sizing Strategy
To ensure correct logic switching threshold ($V_{M} \approx V_{DD}/2$), the Pull-Up Network (PMOS) was sized to be approximately twice the width of the Pull-Down Network (NMOS) for the XOR gate.
* **NMOS Reference:** 0.1µm.
* **PDN Sizing:** 0.2µm (to account for series resistance).
* **PUN Sizing:** 0.4µm (XOR) and 0.2µm (NAND).

### 2. System Architecture
<div align="center">
  <img src="Schematics/2-Bit Adder-Subtractor_Block/Add-Sub_Sch.png" alt="2-Bit Adder Subtractor Schematic" width="800">
</div>

---

## 🔢 Truth Tables & Logic

### 1. System Operation Modes
The circuit behavior is determined by the control signal `M` (connected to $C_{in}$ of the LSB and the XOR inputs).

| Mode ($M$) | Function | Operation | Logic Description |
| :---: | :---: | :---: | :--- |
| **0** | **ADD** | $A + B$ | Standard binary addition. Carry ripples from LSB to MSB. |
| **1** | **SUB** | $A - B$ | 2's Complement Subtraction ($A + \bar{B} + 1$). |

### 2. 1-Bit Full Adder Logic
The 2-bit system is constructed by cascading two of these Full Adder blocks.
<div align="center">

| $A$ | $B$ | $C_{in}$ | Sum ($S$) | $C_{out}$ |
| :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | **0** | **0** |
| 0 | 0 | 1 | **1** | **0** |
| 0 | 1 | 0 | **1** | **0** |
| 0 | 1 | 1 | **0** | **1** |
| 1 | 0 | 0 | **1** | **0** |
| 1 | 0 | 1 | **0** | **1** |
| 1 | 1 | 0 | **0** | **1** |
| 1 | 1 | 1 | **1** | **1** |

</div>
---
## 🧪 Simulation & Results

All simulations were performed in **Synopsys Custom Compiler** using transient analysis.

### Methodology
To validate the logic without writing complex digital vectors, analog voltage sources were used to generate digital patterns.
* **Input A0:** Period $T$
* **Input A1:** Period $2T$
* **Input B0:** Period $4T$
* ...and so on.
This ensures that every possible binary combination is covered in a single transient simulation run.

### Waveforms
The graph below demonstrates the circuit performing both addition and subtraction correctly as the inputs cycle through their states.

<div align="center">
  <img src="Schematics/2-Bit Adder-Subtractor_Block/Add-Sub_TB_Wave.png" alt="Simulation Waveforms" width="800">
  <!-- <p><em>Transient Analysis Output: Green = Inputs, Red = Sum/Difference Output</em></p> -->
</div>

---

## 🚀 Tools & Skills Used
* **Schematic Entry:** Synopsys Custom Compiler.
* **Simulation:** HSPICE / PrimeSim.
* **Waveform Analysis:** PrimeWave.
* **Skills:** Transistor Sizing, Static CMOS Logic, Bottom-Up Verification, Schematic Capture.

---

### 👤 Author
**Kareem AbuSharkh & Nooreldeen Alkomi**
*Princess Sumaya University for Technology - Digital Electronics (21332)*
