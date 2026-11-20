---
title: "Testing Instructions"
description: "Hardware and firmware testing checklist and procedures."
---

Testing procedures for this Slice. Covers manual validation and optional automated checks.

## ✅ Hardware Checklist

- [ ] Visual inspection (solder joints, polarity, orientation)
- [ ] No shorts across power rails
- [ ] All headers and connectors populated

## ⚡ Power-On Verification

1. Apply logic power (e.g., 5V).
2. Confirm LED heartbeat (if present).
3. Measure voltage rails at test points.

## 📟 Functional Tests

| Test        | Method                            | Expected Result                     |
| ----------- | --------------------------------- | ----------------------------------- |
| UART output | Connect USB-Serial, open terminal | Data output at expected baud rate   |
| Input test  | Drive digital input               | MCU registers or reacts accordingly |
| Output test | Scope or multimeter at output     | Output toggles or changes as coded  |

## r2 Checks

Each checks a change made in r2 ([revision plan](revision-r2.md)).

| Test | Method | Expected result |
| --- | --- | --- |
| Input threshold | Sweep `IN1+` to `IN1-` from 0 to 30 V; read D9 | Low below ~7 V, high from 11 V; repeat for IN2–IN4 |
| Reversed input | Apply 24 V with `IN1+` negative | No damage; D9 stays low |
| Input resistor heat | 30 V on an input for 10 min | R3/R5/R7/R9 warm, not hot (0.19 W in a 0.25 W 1206) |
| E-stop interlock | Outputs on, then pull bus pin 6 low | All four relays open; firmware still reads D2 low |
| Field supply reversed | Reverse the 24 V supply on J1 | Nothing conducts, no damage; C8 sees no reverse voltage |
| VFIELD source | JP1 on 1–2, then 2–3 | `VF` terminals at the J1 supply, then at the bus 12 V |
| Wet output | `VF` linked to `OUT1+`, load from `OUT1-` to J1 GND | Load switches with A0 |

## 🔁 Firmware Regression (if present)

```bash
pio run -t test
```

## 📷 Test Setup

<!-- insert annotated image of bench setup under docs/assets/ and insert here -->
