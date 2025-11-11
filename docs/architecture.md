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
| ------ | --------- | --------- | ------- | ----- |
| ...    | ...       | ...       | ...     | ...   |

## 🪛 MCU Pin Mapping

| MCU Pin | Function | Net | Notes |
| ------- | -------- | --- | ----- |
| ...     | ...      | ... | ...   |

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

- Logic power: ...
- High-current domain: ...

## 🧩 Mechanical

- Mounting pattern: ...
- Dimensions: ...
- Notes: ...

---

<!-- Optional: Add block diagrams or images under docs/assets/ -->
