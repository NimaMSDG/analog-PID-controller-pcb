# Analog PID Controller - PCB Design & Simulation

[![Sharif University of Technology](https://img.shields.io/badge/Sharif%20University-EE%20Lab%202-blue.svg)](https://ee.sharif.edu/)
[![Altium Designer](https://img.shields.io/badge/EDA-Altium%20Designer-gold.svg)](https://www.altium.com/)
[![LTspice](https://img.shields.io/badge/Simulation-LTspice-brightgreen.svg)](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)

A complete hardware implementation and simulation of an **Analog Proportional-Integral-Derivative (PID) Controller** based on Operational Amplifiers (Op-Amps). Developed as the final project for the **Electronics Laboratory II (Az Elec 2)** course at **Sharif University of Technology**.

---

## 📌 Project Overview

This repository contains the end-to-end design, schematic capture, PCB layout, and SPICE circuit simulation of a continuous-time analog PID controller. 

The controller processes an error signal $e(t) = r(t) - y(t)$ generated from the difference between the setpoint (reference) and the actual plant output, and produces the corresponding control signal $u(t)$ to drive a feedback system toward stability.

### Key Highlights
- **Op-Amp Topology:** Built using high-precision, low-noise operational amplifiers.
- **Tuneable Gains:** Independent gain control for proportional, integral, and derivative parameters.
- **Noise Mitigation:** Practical real-derivative stage with a high-frequency roll-off pole to suppress high-frequency circuit noise.
- **Integrated Design:** Simulated end-to-end in **LTspice** and engineered as a two-layer printed circuit board in **Altium Designer**.

---

## 📐 Mathematical Formulation

The ideal continuous-time PID algorithm in the time domain is defined as:

$$u(t) = K_p e(t) + K_i \int_0^t e(\tau) d\tau + K_d \frac{de(t)}{dt}$$

Expressed in the Laplace domain (transfer function formulation):

$$C(s) = \frac{U(s)}{E(s)} = K_p + \frac{K_i}{s} + K_d s$$

### Practical Implementation
To prevent infinite gain at ultra-high frequencies in the derivative path, an input low-pass filtering stage is incorporated:

$$C_{\text{practical}}(s) = K_p + \frac{K_i}{s} + \frac{K_d s}{1 + s \frac{K_d}{N}}$$

Where:
- $K_p$: Proportional gain (regulates immediate response speed)
- $K_i$: Integral gain (eliminates steady-state offset error)
- $K_d$: Derivative gain (improves transient phase margin and dampens overshoot)
- $N$: High-frequency filtering coefficient

---

## 🔬 Circuit Architecture & Schematics

The circuit is modularized into four primary analog blocks:
1. **Error Detector / Differential Subtractor:** Computes $e(t) = V_{\text{ref}} - V_{\text{feedback}}$.
2. **Proportional Path (P):** Inverting/non-inverting amplifier with adjustable gain $R_f / R_{in}$.
3. **Integral Path (I):** Active integrator using an op-amp with an RC feedback network.
4. **Derivative Path (D):** Differentiator with a series input resistor-capacitor pair.
5. **Summing Inverter & Buffer:** Aggregates individual channel outputs and normalizes circuit phase.

### Full Schematic Diagram
> *Extracted from course design documentation.*

<p align="center">
  <img src="docs/images/schematics.png" alt="Schematic Diagram" width="850"/>
</p>

---

## 💻 LTspice Simulation Results

Before hardware fabrication, the PID controller was evaluated and tuned across simulated load dynamics in **LTspice**.

### Step Response & Closed-Loop Performance
- **Rise Time ($t_r$):** Significantly reduced via the proportional path.
- **Overshoot ($M_p$):** Suppressed by optimizing the derivative damping factor.
- **Steady-State Error ($e_{ss}$):** Eliminated completely through the integrator branch.

<p align="center">
  <img src="docs/images/step_response.png" alt="LTspice Step Response" width="850"/>
</p>

---

## 🖨️ PCB Design (Altium Designer)

The board layout was carefully crafted to minimize parasitic resistance and stray capacitive coupling:
- **Dedicated Ground Return:** Proper analog return paths to decouple sensitive op-amp feedback loops from supply transients.
- **Decoupling Capacitors:** Local $100\,\text{nF}$ bypass ceramic capacitors adjacent to IC power rails ($V_{CC} / V_{EE}$).
- **Signal Traces:** Short routing paths on high-impedance inverting nodes of the op-amps.

| 2D Layout View | 3D Render |
| :---: | :---: |
| <img src="docs/images/pcb_layout.png" alt="PCB Layout" width="400"/> | <img src="docs/images/pcb_3d.png" alt="PCB 3D View" width="400"/> |

---

## 📂 Repository Structure

```text
.
├── Altium/
│   ├── Sharif_EE2LAB_Project.PrjPcb          # Altium master project file
│   ├── Sharif_EE2LAB_Project.PrjPcbStructure # Project hierarchy metadata
│   ├── Schematics.SchDoc                     # Schematic document
│   └── PCB.PcbDoc                            # Printed circuit board layout
│
├── LTspice/
│   └── *.asc                                 # LTspice circuit schematics and transient runs
│
├── docs/
│   ├── circuit_diagrams.pdf                  # Full compiled documentation & schematics
│   └── images/                               # Graphical assets for documentation
│       ├── schematics.png
│       ├── step_response.png
│       ├── pcb_layout.png
│       └── pcb_3d.png
│
├── .gitignore                                # Ignores Altium/LTspice cache & log files
└── README.md                                 # Main project documentation
```

---

## 🚀 How to Open the Files

### Altium Designer
1. Clone or download this repository:
   ```bash
   git clone https://github.com/NimaMSDG/analog-PID-controller-pcb.git
   ```
2. Open `Altium/Sharif_EE2LAB_Project.PrjPcb` in Altium Designer.
3. Use `3` on your keyboard while in `PCB.PcbDoc` to view the 3D board rendering.

### LTspice
1. Launch LTspice.
2. Navigate to the `LTspice/` directory and open any `.asc` file.
3. Run the transient simulation (`Simulate -> Run`) to inspect loop stability and waveform response.

---

## 👨‍💻 Author & Course Information

- **Student:** Nima (Sharif University of Technology)
- **Course:** Electronics Laboratory II (EE Lab 2)
- **Institution:** Department of Electrical Engineering, Sharif University of Technology
