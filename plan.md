# Rough Plan for SLC_IOCT

**Goal / Description:**

- Create a slice for rugged **digital I/O** control in industrial or field settings.
- Interface directly with **24 V sensors and actuators**.
- Mimics a simple **PLC-style I/O module** for the BREADS system.
- Prioritize **input protection**, reliable switching, and compatibility with common field devices.

**Design Plan:**

- 4–8 opto-isolated digital inputs (24 V-compatible)
- 4–8 protected digital outputs:
  - Open-drain N-channel MOSFETs for low-side switching
  - Onboard SSR relays
- TVS protection, flyback diodes for inductive loads

**Applications:**

- Switches, proximity sensors, and relays in automation systems
- Basic on/off control of 24 V field devices
- Rugged I/O for robotics, process control, or lab automation
