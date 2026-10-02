<div align="center">

# 📈 LTSpice — CMOS Common Source Amplifier Design

### Resistive-load vs. active-load amplifier design: DC biasing, transient gain, and AC frequency response

![LTSpice](https://img.shields.io/badge/Tool-LTSpice-2ea44f?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Analog%20IC%20Design-blue?style=for-the-badge)

![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)
![Devices](https://img.shields.io/badge/Devices-NMOS%20%7C%20PMOS-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

[Experiments](#-experiments) · [Physical Continuity](#-physical-continuity-across-experiments) · [Tools](#-tools--environment) · [Comparative Summary](#-comparative-summary) · [Author](#-author)

</div>

---

## 📌 Overview

This repository documents two LTSpice analog design experiments centered on the **Common Source (CS) amplifier** — the fundamental single-stage MOSFET amplifier building block — using idealized square-law NMOS/PMOS models. The two experiments are structured as a direct comparison: the same basic CS topology is first built with a simple **resistive drain load**, then rebuilt with a **PMOS active load**, isolating exactly how the choice of load transforms the amplifier's gain, output resistance, and area/power trade-offs.

Each experiment includes the **full theoretical derivation**, a **reference LTSpice netlist**, and **detailed simulation results** (DC operating point, transient waveforms, AC Bode plots, and parametric sweeps) compared directly against hand-calculated theory.

---

## 🧪 Experiments

### [Experiment 8 — Common Source Amplifier with Resistive Load](experiment-8-cs-amplifier-resistive-load/)

Designs and simulates a CS amplifier with V_DD = 1.8 V and a 10 kΩ drain resistor, verifying the DC operating point, transient voltage gain (|Av| = gm·RD), and AC frequency response. Includes parametric sweeps of transistor width **W** and drain resistance **R_D**, mapping out exactly where the device leaves saturation and the ideal gain formula breaks down.

<p align="center">
  <img src="experiment-8-cs-amplifier-resistive-load/images/transient_waveform.png" width="45%">
  <img src="experiment-8-cs-amplifier-resistive-load/images/ac_bode_baseline.png" width="45%">
</p>

**Core result:** simulated gain |Av| = 20 (26.02 dB) matches theory exactly at the baseline bias point; gain scales linearly with both W and R_D until the transistor is pushed into triode (W ≥ 41 µm or R_D ≥ 21 kΩ).

### [Experiment 9 — Common Source Amplifier with Active Load](experiment-9-cs-amplifier-active-load/)

Rebuilds the same CS amplifier concept, but replaces the passive drain resistor with a **PMOS transistor biased as a current source (active load)**. Because the PMOS's channel-length-modulation output resistance (r_o2) is far higher than any practical resistor, this dramatically increases voltage gain for the same chip footprint.

<p align="center">
  <img src="experiment-9-cs-amplifier-active-load/images/transient_response.png" width="45%">
  <img src="experiment-9-cs-amplifier-active-load/images/ac_response.png" width="45%">
</p>

**Core result:** measured gain ≈130–133 V/V (42.5 dB) — more than **3× higher** than a comparable resistive-load design — using an effective load resistance of ≈333 kΩ (r_o1 ∥ r_o2) instead of a fixed 10 kΩ resistor.

---

## 🔗 Physical Continuity Across Experiments

Experiment 9 is a direct extension of Experiment 8 — same amplifier topology, same input-referred small-signal analysis, with only the **load element** changed:

| | Experiment 8 (Resistive Load) | Experiment 9 (Active Load) |
|---|---|---|
| **Load element** | Fixed resistor R_D | PMOS transistor (current-source load) |
| **Load resistance** | R_D = 10 kΩ (fixed by component choice) | r_o1 ∥ r_o2 ≈ 333 kΩ (set by bias current & λ) |
| **Gain equation** | Av = −gm·RD | Av = −gm·(ro1 ∥ ro2) |
| **Typical gain achieved** | |Av| = 20 (26.02 dB) | |Av| ≈ 133 (42.5 dB) |
| **Chip area cost** | Large (physical resistor) | Small (transistor only) |
| **Frequency response** | Flat (ideal model, no parasitic caps) | Flat (ideal model, no parasitic caps) |

The two experiments together demonstrate one of the central design trade-offs in analog IC design: **why virtually every real single-stage CMOS amplifier uses an active (transistor) load instead of a physical resistor** — the achievable gain is far higher, and the silicon area is far smaller, at the cost of more careful biasing.

---

## 🛠️ Tools & Environment

| Tool | Purpose |
|---|---|
| **LTSpice** | SPICE-based analog circuit simulator used for DC operating-point, transient, and AC (`.ac`) small-signal analysis |
| **Idealized square-law NMOS/PMOS models** | `.model nmos/pmos NMOS/PMOS(kp=..., VTO=..., lambda=...)` — long-channel models with no parasitic gate capacitances, isolating pure DC/transconductance behavior from high-frequency roll-off effects |

All simulation decks are plain-text SPICE netlists, included in each experiment's `code/` folder — directly importable/reusable in LTSpice without any proprietary schematic file.

---

## 📊 Comparative Summary

| Experiment | Load Type | Load Resistance | Voltage Gain | Gain (dB) |
|---|---|---|---|---|
| 8 — Resistive Load | Fixed resistor R_D | 10 kΩ | 20 V/V | 26.02 dB |
| 9 — Active Load | PMOS current source | ≈333 kΩ (ro1 ∥ ro2) | ≈133 V/V | 42.5 dB |

Both experiments confirm the same underlying small-signal model (`Av = −gm·Rload`, 180° phase inversion, flat frequency response for a capacitance-free ideal model) — the only variable is what physically constitutes `Rload`, and how much higher that resistance can be pushed by using transistor output resistance instead of a discrete component.

---

## 📁 Repository Structure

```
LTSpice-CMOS-Amplifier-Design/
├── README.md
├── LICENSE
├── .gitignore
├── experiment-8-cs-amplifier-resistive-load/
│   ├── README.md
│   ├── code/
│   │   └── cs_amplifier_resistive_load.cir
│   └── images/ (8 files)
└── experiment-9-cs-amplifier-active-load/
    ├── README.md
    ├── code/
    │   └── cs_amplifier_active_load.cir
    └── images/ (3 files)
```

---

## 👨‍💻 Author

<div align="center">

**Divyanshu Kumar**

B.Tech (Electronics & Communication Engineering) · University of Delhi

[![Portfolio](https://img.shields.io/badge/Portfolio-gignova.netlify.app-000000?style=for-the-badge)](https://gignova.netlify.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/divyanshu-kumar-7625a8315)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Divyanshukumar2005)

</div>

---

<div align="center">

⭐ **If you found this project useful or interesting, consider giving it a star!**

</div>
