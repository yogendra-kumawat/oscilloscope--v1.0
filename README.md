# ⚡ Function_generator v1.0 — Power & Control Board

![Version](https://img.shields.io/badge/version-1.0-blue)
![Type](https://img.shields.io/badge/type-Hardware-orange)
![Status](https://img.shields.io/badge/status-active-brightgreen)
![MCU](https://img.shields.io/badge/MCU-STM32F103-red)
![RTOS](https://img.shields.io/badge/RTOS-FreeRTOS%20%2F%20CMSIS--OS2-yellow)

> Battery-powered portable oscilloscope — power management, voltage regulation, user control interface board, and STM32-based firmware with real-time waveform generation and display.

---

## 📋 Overview

This project is a **portable open-source Function_generator** built around the STM32F103 microcontroller. It combines:

- Safe **Li-ion battery charging** from an external USB/DC source
- A clean **regulated voltage rail** for the microcontroller (MCU)
- **Control switches** for power on/off and user interaction
- Protection and filtering circuitry for stable operation
- **Real-time waveform generation** with adjustable frequency and duty cycle **{PWM ,DC,Square}{working for sine,triangular,and oscilloscopic function}**
- **Dual-channel ADC signal acquisition** with interrupt-driven sampling
- **SSD1306 OLED display** with a multi-window navigation menu
- **FreeRTOS / CMSIS-OS2** multitasking firmware

---

## 🔲 System Block Diagram

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  USB / DC   │────▶│  Battery     │────▶│  Li-ion /   │
│  Input (5V) │     │  Charger IC  │     │  LiPo Cell  │
└─────────────┘     └──────────────┘     └──────┬──────┘
                                                 │
                                                 ▼
                    ┌──────────────┐     ┌───────────────┐
                    │   STM32F103  │◀────│   Voltage     │
                    │   (MCU)      │     │   Regulator   │
                    └──────┬───────┘     └───────────────┘
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
   ┌─────────────┐  ┌────────────┐  ┌─────────────┐
   │ ADC1 / ADC2 │  │ TIM1 / TIM2│  │ SSD1306     │
   │ Signal In   │  │ PWM Out    │  │ OLED (I2C)  │
   └─────────────┘  └────────────┘  └─────────────┘
          ▲
   ┌─────────────┐
   │  Control    │
   │  Switches   │
   │ (EXTI GPIO) │
   └─────────────┘
```

## Battery IC

<img width="746" height="360" alt="image" src="https://github.com/user-attachments/assets/482b9a10-772b-4144-b45c-be53a566b9e6" />

## Controller Regulator

<img width="544" height="317" alt="image" src="https://github.com/user-attachments/assets/dcc41ac5-b95d-4321-bfda-617e66328937" />

## Full Circuit

<img width="466" height="341" alt="image" src="https://github.com/user-attachments/assets/5cd9beef-68f7-453a-b8dc-805cf51b11f8" />

---

## 🧩 Key Hardware Components

| Module | Function | Interface | Type |
|---|---|---|---|
| **Battery Charger IC** | CC/CV Li-ion charging; over-voltage & over-current protection | USB / DC jack | Power |
| **Li-ion / LiPo Cell** | Main energy storage; powers device off-grid | JST connector | Power |
| **Voltage Regulator** | Converts battery voltage to stable 3.3 V / 5 V for MCU | EN pin (switch-controlled) | Regulation |
| **Power Switch** | Hard on/off; drives regulator enable to cut standby draw | Mechanical toggle | Control |
| **Mode / Select Switch** | User input for menu navigation; connects to MCU GPIO via EXTI | MCU GPIO (EXTI2/3) | Control |
| **SSD1306 OLED** | 128×32 display for menus, waveform preview, freq & duty info | I2C (I2C1, 100 kHz) | Display |
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
| MCU | STM32F103 @ 48 MHz (HSE × PLL ×6) |
| ADC Resolution | 12-bit (0–4095) |
| ADC Channels | ADC1 — CH0 (coarse), ADC2 — CH1 (fine) |
| PWM Output | TIM1 CH2 (variable freq + duty) |
| UART | USART1 @ 9600 baud |

---

## 🖥️ Firmware Architecture

The firmware runs on **FreeRTOS / CMSIS-OS2** with four concurrent tasks:

| Task | Priority | Role |
|---|---|---|
| `wave_gen` | Normal | Applies selected waveform output via `set_wave()` |
| `read_wave` | Normal | Reserved for signal acquisition / processing |
| `display` | Low | Drives SSD1306 OLED menu and waveform preview |
| `monitor` | Realtime | Polls ADC via interrupt, handles button EXTI events |

### Clock Configuration

```
HSE (external crystal)
  └──▶ PLL ×6 → 48 MHz SYSCLK
         ├── AHB  /1 → 48 MHz
         ├── APB1 /2 → 24 MHz  (TIM2, I2C)
         ├── APB2 /1 → 48 MHz  (TIM1, ADC)
         ├── ADC  /6 →  8 MHz
         └── USB      → 48 MHz (direct PLL)
```

---

## 📟 Display Menu System

The OLED display (`window` variable) has five screens:

| Window | Screen | Description |
|---|---|---|
| `0` | **Main Menu** | Scrollable list — Waves, Oscill., Port, PWM, Config |
| `1` | **Wave Select** | Choose waveform type: Sine, Ramp, Square, Noise, DC |
| `3` | **Port Select** | Choose output port: A or B |
| `4` | **PWM Control** | Live freq + duty cycle adjust with waveform preview |
| `5` | **Config** | Register/settings placeholder |

Navigation uses two buttons:
- **SELECT\_1** (`EXTI2`) — scroll / increment selection
- **SELECT\_2** (`EXTI3`) — confirm / enter submenu (toggle `select2`)
## display overview
<img width="400" height="225" alt="WhatsApp Video 2026-06-03 at 1 54 03 PM" src="https://github.com/user-attachments/assets/6116fe94-ce2e-4ae8-816e-3183127bfa71" />
<img width="400" height="225" alt="WhatsApp Video 2026-06-03 at 1 50 50 PM" src="https://github.com/user-attachments/assets/5aeb9037-2c02-4576-ba21-5775c3f00c33" />

---

## 🌊 PWM / Waveform Generation

`set_dynamic_frequency()` computes TIM1 prescaler and auto-reload register values at runtime to hit any target frequency from the 48 MHz timer clock:

```c
uint32_t total_divider = 48_000_000 / target_freq;
uint32_t psc = (total_divider - 1) / 65536;
uint32_t arr = (total_divider / (psc + 1)) - 1;
uint32_t pulse = (duty_cycle * (arr + 1)) / 100;
```

- **Frequency range:** ~1 Hz to ~47 MHz (capped at 47 MHz in MHz mode)
- **Duty cycle:** 0–100%, derived from ADC1 coarse potentiometer
- **Fine frequency:** derived from ADC2 fine potentiometer
- **Frequency units:** selectable — Hz / kHz / MHz (multipliers: ×1 / ×1000 / ×1 000 000)

---

## 🔍 How It Works

### 1 — Battery Charging

When a USB or DC supply is connected, based on feedback from charging battery the power is controlled ,basically turning off the power supply when battery is charged.

### 2 — Regulated Power to MCU

basically is is based on the voltage follower principle.

### 3 — Signal Acquisition (ADC)

Both ADC1 and ADC2 are started in **interrupt mode** (`HAL_ADC_Start_IT`) by the `monitor` task every 300 ms(also to control the bouncing effect of buttons so giving delay ). The `HAL_ADC_ConvCpltCallback` ISR:
- Reads the raw 12-bit value
- Tracks rolling **min / max** for dynamic range normalisation
- Toggles an indicator GPIO (`indi_Pin`) on every conversion

### 4 — Control Switches (EXTI)

`select_1_Pin` and `select_2_Pin` are configured as **falling-edge EXTI** inputs with internal pull-ups. The EXTI callback sets `flag_1` and wakes the `monitor` task via `osThreadFlagsSet`. The monitor task then increments `select1` (0–4) or `select2` (0–1) accordingly.

---

## 🚀 Getting Started

**Step 1 — Connect Battery**  
Connect a Li-ion / LiPo cell to the JST battery connector, observing polarity markings on the PCB silkscreen.

**Step 2 — (Optional) Charge**  
Connect a 5 V USB or DC supply. The onboard charging IC will manage the charge cycle automatically. The charge LED illuminates during active charging.

**Step 3 — Flash Firmware**  
Open the project in **STM32CubeIDE**{extract the file and arrange accordigly in stm ide as due to version conflicts the direct project will not work }.
see main.c file for full logic implementation

**Step 4 — Power On**  
Flip the power switch to **ON**. The voltage regulator enables, the MCU boots, and the OLED displays the main menu.

**Step 5 — Navigate**  
- Press **SELECT\_1** to scroll through menu items.
- Press **SELECT\_2** to enter a submenu.
- In **PWM mode** (Window 4): rotate the coarse pot (ADC1) to adjust duty cycle; rotate the fine pot (ADC2) to adjust frequency. Press SELECT\_1 to cycle Hz → kHz → MHz.

---

## 🛠️ Dependencies

| Dependency | Purpose |
|---|---|
| STM32CubeIDE / CubeMX | Project generation and build toolchain |
| STM32 HAL (F1 series) | Peripheral abstraction (ADC, TIM, I2C, UART, USB) |
| CMSIS-OS2 / FreeRTOS | RTOS kernel |
| `ssd1306` driver | I2C OLED display library (`ssd1306.h`, `ssd1306_fonts.h`) |{references->https://github.com/afiskon/stm32-ssd1306}

---

## 📄 License

This project is open source. See [`LICENSE`](./LICENSE) for details.

---

