# ⚡ Oscilloscope v1.0 — Power & Control Board

![Version](https://img.shields.io/badge/version-1.0-blue)
![Type](https://img.shields.io/badge/type-Hardware-orange)
![Status](https://img.shields.io/badge/status-active-brightgreen)

> Battery-powered portable oscilloscope — power management, voltage regulation, and user control interface board.

---

## 📋 Overview

This board forms the **power backbone** of the Oscilloscope v1.0 project. It handles:

- Safe **Li-ion battery charging** from an external USB/DC source
- A clean **regulated voltage rail** for the microcontroller (MCU)
- **Control switches** for power on/off and user interaction
- Protection and filtering circuitry for stable operation

---

## 🔲 Block Diagram

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  USB / DC   │────▶│  Battery     │────▶│  Li-ion /   │
│  Input (5V) │     │  Charger IC  │     │  LiPo Cell  │
└─────────────┘     └──────────────┘     └──────┬──────┘
                                                 │
                                                 ▼
                    ┌──────────────┐     ┌───────────────┐
                    │ Microcontrol-│◀────│   Voltage     │
                    │ ler (MCU)    │     │   Regulator   │
                    └──────────────┘     └───────────────┘
                           ▲
                           │
                    ┌──────────────┐
                    │   Control    │
                    │   Switches   │
                    └──────────────┘
```
## battery ic

<img width="746" height="360" alt="image" src="https://github.com/user-attachments/assets/482b9a10-772b-4144-b45c-be53a566b9e6" />

## controller regulator
<img width="544" height="317" alt="image" src="https://github.com/user-attachments/assets/dcc41ac5-b95d-4321-bfda-617e66328937" />

## full circuit

<img width="466" height="341" alt="image" src="https://github.com/user-attachments/assets/5cd9beef-68f7-453a-b8dc-805cf51b11f8" />

---

## 🧩 Key Components

| Module | Function | Interface | Type |
|---|---|---|---|
| **Battery Charger IC** | CC/CV Li-ion charging; over-voltage & over-current protection | USB / DC jack | Power |
| **Li-ion / LiPo Cell** | Main energy storage; powers device off-grid | JST connector | Power |
| **Voltage Regulator** | Converts battery voltage to stable 3.3 V / 5 V for MCU | EN pin (switch-controlled) | Regulation |
| **Power Switch** | Hard on/off; drives regulator enable to cut standby draw | Mechanical toggle | Control |
| **Mode / Reset Switch** | User input for mode selection; connects to MCU GPIO | MCU GPIO | Control |
| **Decoupling Capacitors** | Filters noise on power rails; stabilises regulator output | Inline on VCC / GND | Passive |

---

## ⚙️ Electrical Specifications

| Parameter | Value |
|---|---|
| Input Voltage | 5 V (USB / DC) |
| Battery Type | Li-ion / LiPo |
| Battery Voltage | 3.7 V nominal |
| Regulated Output | 3.3 V / 5 V |
| Max Charge Current | 500 mA (configurable) |
| Standby Quiescent Current | < 1 µA (regulator disabled) |

---

## 🔍 How It Works

### 1 — Battery Charging

When a USB or DC supply is connected, the charging IC enters **constant-current (CC)** mode to bulk-charge the Li-ion cell, then transitions to **constant-voltage (CV)** mode near full capacity. Built-in protection prevents overcharge, over-discharge, and short-circuit faults.

### 2 — Regulated Power to MCU

The battery output feeds a low-dropout (LDO) or switching voltage regulator. The regulator's **enable (EN) pin** is tied to the power switch — turning the switch off immediately cuts the MCU rail, minimising battery drain. Decoupling capacitors on the output suppress switching noise and ensure a clean supply.

### 3 — Control Switches

A **main power toggle** drives the regulator enable pin for hard on/off. Additional tactile or slide switches route directly to **MCU GPIO lines**, allowing firmware to detect user input for mode selection, triggering, or calibration. The MCU's internal pull-ups are used — no external resistors needed.

---

## 🚀 Getting Started

**Step 1 — Connect Battery**
Connect a Li-ion / LiPo cell to the JST battery connector, observing polarity markings on the PCB silkscreen.

**Step 2 — (Optional) Charge**
Connect a 5 V USB or DC supply. The onboard charging IC will manage the charge cycle automatically. The charge LED illuminates during active charging.

**Step 3 — Power On**
Flip the power switch to **ON**. The voltage regulator enables and the MCU powers up, initiating the oscilloscope firmware.

**Step 4 — Use Controls**
Use the mode and control switches to navigate the oscilloscope interface as described in the firmware documentation.

---

## 📁 Repository Structure

```
oscilloscope--v1.0/
├── schematics/        # Circuit schematics (PDF / KiCad)
├── pcb/               # PCB layout files
├── firmware/          # MCU firmware source
└── README.md
```

---

## 📄 License

This project is open source. See [`LICENSE`](./LICENSE) for details.

---

*Made by [yogendra-kumawat](https://github.com/yogendra-kumawat)*
