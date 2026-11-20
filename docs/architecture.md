---
title: "Architecture Overview"
description: "High-level architecture: electrical, firmware, and mechanical design notes."
---

Overview of the electrical, firmware, and mechanical design of this Slice.

## Rough Plan for SLC_IOCT

TODO: remove this and disperse relevant info into appropriate sections below as they are fleshed out.

**Goal / Description:**

- Create a slice for rugged **digital I/O** control in industrial or field settings.
- Interface directly with **24 V sensors and actuators**.
- Mimics a simple **PLC-style I/O module** for the BREADS system.
- Prioritize **input protection**, reliable switching, and compatibility with common field devices.

**Design Plan:**

- 4–8 opto-isolated digital inputs (24 V-compatible)
- 4–8 protected digital outputs using onboard SSR relays
- TVS protection, flyback diodes for inductive loads

**Applications:**

- Switches, proximity sensors, and relays in automation systems
- Basic on/off control of 24 V field devices
- Rugged I/O for robotics, process control, or lab automation

## 🧠 Purpose

<!-- Briefly describe the function of the Slice -->

This Slice is designed to be a rugged digital input and output slice that can interface directly with 24V industrial sensors and actuators. It mimics a PLC-style IO module for the BREADS system. It prioritizes input protection, reliable switching, and compatibility with common field devices.

## 🔌 Interfaces

| Signal | Connector | Direction | Voltage | Notes |
| --- | --- | --- | --- | --- |
| IN1–IN4 | J3 (`INx+`, `INx-`, `GND` per channel) | Input | 12–30 V | Optocoupler; OFF below ~7 V, ON from 11 V |
| OUT1–OUT4 | J2 (`OUTx-`, `OUTx+`, `VF` per channel) | Output | ≤30 V DC | Photo-MOSFET relay, 0.7 A, TVS across each |
| +24 V in, 0 V | J1 pins 1–2 | Power in | 24 V (≤30 V) | Reverse-polarity protected, TVS |
| GND (load return) | J1 pins 3–4 | Power | — | Return for wet-contact loads |
| VFIELD source | JP1 | — | — | 1–2 external 24 V, 2–3 bus 12 V; 2 A total |
| E-stop | Bus pin 6 | Input | 5 V = run | Low turns every output off (Q2) |

## 🪛 MCU Pin Mapping

| MCU Pin | Function | Net | Notes |
| --- | --- | --- | --- |
| D9 | IN1 | IN1 | High when the input is on |
| D8 | IN2 | IN2 | |
| D7 | IN3 | IN3 | |
| D6 | IN4 | IN4 | |
| A0 | OUT1 | OUT1 | High switches the relay on |
| A1 | OUT2 | OUT2 | |
| A2 | OUT3 | OUT3 | |
| A3 | OUT4 | OUT4 | |
| D2 | E-stop | ESTOP via R16 (1 k) | Low = stop; read without the internal pull-up |
| D5 | Status LED | LED | WS2812B data |
| A4, A5 | I²C | via JP2–JP3 | Solder jumpers JP2–JP5 bridged = I²C (default); cut all four for CAN |

### DUMP

- Supported MCU Dev Boards:
  - Arduino Nano Original (ATMega328P)
  - Arduino Nano Every (ATMega4809)
  - STM32 Nucleo-32 series (e.g. L432KC, F303K8)
  - Other Nano-format boards also supported:
    - Any MCU board using the standard Arduino Nano pinout and 0.1" dual-row 15x2 header format
    - Nano ESP32 and ESP32-S2 variants with Nano footprint (e.g. Nano ESP32 by Arduino, or 3rd party clones)
    - RP2040 Nano-form boards (e.g. Nano RP2040 Connect, Seeed Studio RP2040 Nano)
    - CH32V203 or CH32V003 Nano-format RISC-V dev boards

## 🔋 Power Domains

- Logic: 5 V from the AP63205 buck on the bus 12 V.
- Field: VFIELD, from the external 24 V on J1 or the bus 12 V (JP1), 2 A
  total.

## 🧩 Mechanical

- Mounting pattern: ...
- Dimensions: ...
- Notes: ...

---

<!-- Optional: Add block diagrams or images under docs/assets/ -->
