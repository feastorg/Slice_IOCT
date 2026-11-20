---
title: "Revision r2 Plan"
description: "What r2 changes on the IOCT board, and why."
---

r2 keeps the G3 board: the 10-pin Slice Interface 1 bus, 70 × 100 mm, two
layers. It fixes the circuit faults found in a review of r1 (`R1 01-2026` on
the silkscreen) and makes the board ready to fabricate.

## Faults in r1

| # | Fault | Evidence |
| --- | --- | --- |
| 1 | The input series resistors (R3, R5, R7, R9: 1 kΩ, 0603) dissipate 0.52 W at 24 V and 0.11 W at 12 V; an 0603 is rated about 0.1 W. | (24 − 1.2 V)² / 1 kΩ |
| 2 | A reversed field input puts the full input voltage across the optocoupler LED, rated 6 V reverse. | PC817 datasheet, VR = 6 V |
| 3 | SW3 connects the Nano's output pins (A0–A3) to the bus e-stop line. The line is 5 V while running, so an output driven low pulls it toward 0 V: it shorts the chain's supply and can stop the whole system. | `bread-infra` `spec/slice/firmware.md`, E-stop polarity |
| 4 | C8, an electrolytic, sits on the input side of the reverse-polarity MOSFET, so a reversed supply reverse-biases it. | Input Power Protection sheet |
| 5 | SMAJ24CA starts conducting at 26.7 V, below the 30 V top of a 24 V industrial supply, and clamps at 38.9 V, above Q1's 30 V rating (NTGS4111P). | Vishay SMAJ table; onsemi NTGS4111P |
| 6 | Nothing limits the voltage across an output relay when an inductive load turns off; the CPC1002N blocks 60 V. | CPC1002N R09 |
| 7 | SW1 (Würth WS-SLTV 450301014042) is rated 300 mA at 24 V DC, but four 0.7 A outputs can draw 2.8 A through it. | WS-SLTV datasheet |
| 8 | A load switched from VFIELD has no ground terminal to return to except J1 pin 2. | Root sheet |
| 9 | The output net names disagree: OUT1 calls the relay's DC+ "OUT1+", OUT2–OUT4 call it "OUTx−". The copper is the same on all four. | CPC1002N R09 pinout: pin 4 DC+, pin 3 DC− |

## Changes

### Inputs (×4): 12–24 V with a switching threshold

Field input → 2.7 kΩ 1206 → 6.8 V zener (MM3Z6V8) → optocoupler LED, with a
1N4148WS across the LED (cathode to anode) so a reversed input is held to one
diode drop. The emitter pull-down goes from 1 kΩ to 22 kΩ.

| Input | LED current | 2.7 kΩ dissipation |
| --- | --- | --- |
| below 7.4 V (6.4 V zener min + 1.0 V LED) | none: OFF | — |
| 11 V, worst case (7.2 V zener, 1.4 V LED) | 0.89 mA | 2 mW |
| 24 V, typical | 5.9 mA | 95 mW |
| 30 V, worst case (6.4 V zener, 1.0 V LED) | 8.4 mA | 0.19 W of the 1206's 0.25 W |

At 11 V the output must reach the ATmega328P's 3.0 V input-high level
(0.6 × VCC): with 22 kΩ that needs 0.14 mA of collector current, a CTR of 15 %
at 0.89 mA (*estimate*; the PC817 minimum is 50 % at 5 mA, and CTR falls at
lower currents). This gives IEC 61131-2 Type 3-style thresholds (OFF at
≤ 5 V, ON at ≥ 11 V). It does not meet Type 3's 2 mA minimum ON current at
11 V, and it is not certified.

### E-stop interlock replaces SW3

R10–R13 (the relay LED resistors) return through one N-MOSFET (2N7002) instead
of straight to GND. Its gate comes from the bus e-stop line through 1 kΩ, with
100 kΩ to GND. While the line is at the 5 V run level, the relays work as
before; when it drops low, or the slice is unplugged, every output turns off,
whatever the firmware does. The Nano still reads the line on D2. SW3 is
removed.

LED current through the relay: (5 − 1.36 V) / 1.3 kΩ = 2.8 mA, above the
CPC1002N's 2 mA maximum operate current. The 2N7002's on-resistance at 5 V
gate drive adds well under 0.1 V for the 11 mA of four relays.

### Input power path

- Q1 becomes a DMP6023LE: −60 V, ±20 V gate, 28 mΩ at −10 V, SOT-223.
- D4 becomes an SMAJ30CA: 30 V standoff, 33.3 V minimum breakdown, 48.4 V
  clamp, below Q1's 60 V.
- C8 moves to the protected side (24V_OUT), rated 63 V.
- D3 (18 V zener, 19.1 V maximum) still holds Q1's gate within ±20 V.

### Outputs (×4)

- An SMF33A TVS across each relay (cathode on DC+): 33 V standoff, above the
  30 V maximum VFIELD; 53.3 V clamp, below the relay's 60 V.
- Nets renamed so OUTx+ is DC+ (pin 4) and OUTx− is DC− (pin 3) on every
  channel. Wet contact: link VFIELD to OUTx+ (the next terminal), and wire the
  load between OUTx− and GND.

### VFIELD selector and field ground

- SW1 becomes a 1×3 2.54 mm header with a shunt: bus +12V, VFIELD, external
  +24V. VFIELD is documented as 2 A total, the rating class of a standard
  shunt. Taking VFIELD from the bus loads the backplane's 12 V supply.
- J1 becomes the 4-position part of the same Phoenix PTSA 1.5 3.5 mm series:
  +24V in, GND in, and two GND returns for loads.
- VFIELD and +24V copper is sized for 2 A.

### Layout and documentation

- Terminal labels on the silkscreen for J1, J2 and J3, and a revision mark of
  `R2`.
- Output and signal tracks at least 0.5 mm (the G3 design constraint).
- `slice.yaml`, `README.md`, `docs/architecture.md` and `docs/testing.md`
  brought in line with the board: 4 inputs, 4 outputs, the real connector
  references and the pin map.
- ERC and DRC clean, including the two dangling I2C labels.

## Still open

- The CPC1002N is marked "Not For New Designs" (R09). r2 keeps it unless a
  drop-in replacement is found.
- 2N7002, 1N4148WS and SMF33A ratings are taken from their standard series
  values and still to be read from a primary datasheet.
- The Phoenix order number for the 4-position J1.
- Per-channel status LEDs, for a later revision.
