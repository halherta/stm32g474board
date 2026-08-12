# EmbLynx

An embedded systems development board based on the **STM32G474RE** microcontroller.

![EmbLynx board](https://github.com/halherta/stm32g474board/blob/main/images/EmbLynx_photo.jpg)

EmbLynx is a compact, feature-rich development board built around ST's STM32G474RE — a mixed-signal Cortex-M4 SoC with an unusually dense analog front end (5x ADC, 7x DAC, 6x OPAMP, 7x comparator) alongside DSP/math accelerators aimed at digital power conversion, motor control, and precision sensing. The board exposes that analog and digital peripheral set on standard 0.1" headers so it can be used as a general-purpose prototyping platform or stacked with daughterboards.

Full write-up: [hussamtalkstech.com/emblynx](https://hussamtalkstech.com/emblynx/)

## Features

- **MCU:** STM32G474RE (Arm Cortex-M4F, up to 170 MHz, 512 KB Flash, 128 KB SRAM)
- **Power input:**
  - Reverse-polarity-protected DC jack or Vin pin (6–30 V), stepped down via an AP63205WU buck converter to 5 V
  - Direct 5 V input via I/O header pins or USB-C (with TVS protection and 22 Ω series resistors)
  - 3.3 V rail generated from the 5 V rail via an LDO
- **Debug:** STDC14 SWD/JTAG/serial port (STLink-V3 compatible)
- **User I/O:** RESET and BOOT0 buttons (USB DFU-friendly), user pushbutton (PC13), power LED, user LED (PD2)
- **Analog support:** Jumper-selectable Vref (+3V3 or external), filtering on VDD_A / VREF+
- **Current measurement:** IDD jumper (JP2) isolates the microcontroller's own 3.3 V rail (+3V3) from the peripheral/LED rail (+3V30) for clean current measurements
- **RTC backup:** BAT54C dual-Schottky arrangement feeds VBAT from either a coin-cell on the I/O header or +3V3 when no battery is present
- **Form factor:** 2.2" x 3.3", 4-layer PCB, 6x 8-pin 0.1" I/O headers (stacking-header compatible)

## STM32G474RE peripheral highlights

| Category | Details |
|---|---|
| Core | Cortex-M4F w/ FPU + DSP, 170 MHz, ART Accelerator, 0-wait-state Flash |
| Memory | 512 KB dual-bank Flash (ECC), 128 KB SRAM (parity on select blocks), 32 KB CCM-SRAM |
| Math accelerators | CORDIC (trig/hyperbolic), FMAC (FIR/IIR/3p3z filters) |
| Timers | 17 total, incl. HRTIM (184 ps resolution), motor-control timers, watchdogs |
| Connectivity | 3x FDCAN, 4x I2C (Fast-mode+), 5x USART/UART, 1x LPUART, 4x SPI/2x I2S, SAI, USB 2.0 FS, USB-C/UCPD |
| ADC | 5x independent 12-bit SAR, up to 4 Msps each (8 Msps interleaved), up to 16-bit via oversampling |
| DAC | 7 channels (3 buffered external @ 1 Msps, 4 unbuffered internal @ 15 Msps) |
| OPAMP | 6x, externally accessible, configurable as PGAs (x2–x64), ~13 MHz bandwidth |
| Comparator | 7x, 16.7 ns propagation delay, programmable hysteresis |
| Reference | VREFBUF with 2.048 V / 2.5 V / 2.95 V presets |
| Other | 16-channel DMA, FSMC/Quad-SPI, hardware RNG, CRC, internal temp sensor |


## Getting started

### Prerequisites

- [KiCad](https://www.kicad.org/) 7.x or later

### Opening the project

1. Clone this repository:
   ```bash
   git clone https://github.com/halherta/stm32g474board.git
   ```
2. Open `hardware/emblynx.kicad_pro` in KiCad.

### Schematic

![EmbLynx board schematic](https://github.com/halherta/stm32g474board/blob/main/images/schematic.svg)

A rendered PDF of the full schematic is included [here](https://github.com/halherta/stm32g474board/blob/main/stm32g474board.pdf).


### Layout

![EmbLynx board layout](https://github.com/halherta/stm32g474board/blob/main/images/layout.png)

### Parts Library

my latest Kicad footprint / symbol parts library cab always be found [here](https://github.com/halherta/mykicadlibrary)

## Power architecture

| Rail | Source | Notes |
|---|---|---|
| VIN (6–30 V) | DC jack or I/O header | Reverse-polarity protected |
| +5V | VIN → AP63205WU buck, or direct 5 V pin, or USB-C | USB-C input has TVS + 22 Ω series protection |
| +3V30 | +5V → LDO | Powers LEDs, pull-ups, always-on peripherals |
| +3V3 | +3V30 → IDD jumper (JP2) | Powers the MCU only; JP2 allows current measurement of the MCU in isolation |
| VBAT | Coin cell (I/O header) or +3V3 | Selected via BAT54C dual-Schottky diodes |



## Credits

Designed by [Hussam](https://hussamtalkstech.com/about) — [hussamtalkstech.com](https://hussamtalkstech.com/)
