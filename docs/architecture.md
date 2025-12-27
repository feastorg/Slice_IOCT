---
title: "Architecture Overview"
description: "High-level architecture: electrical, firmware, and mechanical design notes."
---

Overview of the electrical, firmware, and mechanical design of this Slice.

## 🧠 Purpose

<!-- Briefly describe the function of the Slice -->
This Slice is designed to be a rugged digital input and output slice that can interface directly with 24V industrial sensors and actuators. It mimics a PLC-style IO module for the BREADS system. It prioritizes input protection, reliable switching, and compatibility with common field devices.

## 🔌 Interfaces

| Signal | Connector  | Direction | Voltage | Notes               |
| ------ | ---------- | --------- | ------- | ------------------- |
| IN1    | Screw down | Input     | 24V     | High voltage input  |
| IN2    | Screw down | Input     | 24V     | High voltage input  |
| IN3    | Screw down | Input     | 24V     | High voltage input  |
| IN4    | Screw down | Input     | 24V     | High voltage input  |
| IN5    | Screw down | Input     | 24V     | High voltage input  |
| IN6    | Screw down | Input     | 24V     | High voltage input  |
| OUT1   | Screw down | Output    | 12V/24V | High voltage output |
| OUT2   | Screw down | Output    | 12V/24V | High voltage output |
| OUT3   | Screw down | Output    | 12V/24V | High voltage output |
| OUT4   | Screw down | Output    | 12V/24V | High voltage output |

## 🪛 MCU Pin Mapping

| MCU Pin | Function | Net  | Notes |
| ------- | -------- | ---- | ----- |
| D6      | IN1      | IN1  |       |
| D7      | IN3      | IN3  |       |
| D8      | IN5      | IN5  |       |
| D9      | IN6      | IN6  |       |
| D10     | IN4      | IN4  |       |
| D11     | IN2      | IN2  |       |
| A0      | OUT1     | OUT1 |       |
| A1      | OUT2     | OUT2 |       |
| A2      | OUT3     | OUT3 |       |
| A3      | OUT4     | OUT4 |       |

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

- Logic power: 5V
- High-current domain: 12V/24V

## 🧩 Mechanical

- Mounting pattern: ...
- Dimensions: ...
- Notes: ...

---

<!-- Optional: Add block diagrams or images under docs/assets/ -->
