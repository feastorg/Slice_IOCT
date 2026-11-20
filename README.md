# Slice_IOCT

Industrial digital I/O slice for BREAD: four 12–24 V inputs and four
solid-state relay outputs, for sensors and actuators on a 24 V field supply.

An Arduino Nano on the 10-pin slice bus (G3, Slice Interface 1), 70 × 100 mm,
two layers. Hardware revision r2.

## Introduction

The slice works like a small PLC I/O module. Each input reads a field signal
through an optocoupler with a switching threshold; each output is a photo-MOSFET
relay that switches a load from the field supply or acts as a dry contact. A low
level on the bus e-stop line turns every output off in hardware, whatever the
firmware does.

## Usage

### Inputs (J3)

Each input has three terminals: `INx+`, `INx-` and `GND`.

- **Floating (isolated):** connect the sensor's output between `INx+` and
  `INx-`. The source must provide its own return path.
- **Board-referenced:** link `INx-` to the `GND` terminal beside it and drive
  `INx+` against board ground.

An input reads OFF below about 7 V and ON from 11 V; it is rated to 30 V. A
reversed input does no harm. The Nano reads IN1–IN4 on D9, D8, D7 and D6, high
when the input is on.

### Outputs (J2)

Each output has three terminals: `OUTx-`, `OUTx+` and `VF` (VFIELD).

- **Wet contact:** link `VF` to `OUTx+` (the next terminal) and wire the load
  from `OUTx-` to `GND` on J1.
- **Dry contact:** leave `VF` unconnected and use `OUTx+` / `OUTx-` as a DC
  switch, `OUTx+` the more positive, up to 30 V.

Each output is rated 0.7 A and has a unidirectional TVS diode across it (33 V
standoff), which is why the outputs switch DC only. Fit a flyback diode
across an inductive load anyway. The Nano drives OUT1–OUT4 on A0–A3, high to
switch on.

### Field supply (J1) and VFIELD (JP1)

| J1 pin | Use |
| --- | --- |
| 1 | +24 V field supply in |
| 2 | 0 V of the field supply |
| 3, 4 | GND, return for wet-contact loads |

The 24 V input has reverse-polarity protection and a 30 V TVS diode. JP1 picks
where VFIELD comes from:

| JP1 shunt | VFIELD |
| --- | --- |
| 1–2 | the external supply on J1 |
| 2–3 | the bus +12 V |

VFIELD is limited to 2 A in total. Taking it from the bus loads the backplane's
12 V supply.

### E-stop

Bus pin 6 is the e-stop line (5 V = run, low = stop). It gates the relay drive
through Q2, so a stop, or a slice pulled from the bus, turns all four outputs
off. The Nano reads the same line on D2.

## Documentation

See [docs](docs/), including the [r2 revision plan](docs/revision-r2.md).

---

<div align="center">
  <p>
    <img src="https://img.shields.io/badge/Developed_by-UWO--FAST--et--al-orange" alt="Developed by UWO-FAST et al.">
    <img src="https://img.shields.io/badge/Powered_by-Open_Source-blue" alt="Powered by Open Source">
  </p>
</div>
