# TCA9406 Breakout Board — Rev A

**Ivory Electronics** | KiCAD 9.0 | 2-Layer PCB

A compact breakout board for the Texas Instruments **TCA9406DCUR**, a 2-bit bidirectional I2C/SMBus voltage-level translator with 8-kV HBM ESD protection. Designed to bridge two I2C buses operating at different voltage levels (e.g., 1.8V ↔ 3.3V, 3.3V ↔ 5V) with onboard optional pull-up resistors for rapid prototyping.

---

## Key Specifications

| Parameter | Value |
|---|---|
| Main IC | TCA9406DCUR (VSSOP-8, 2.3×2 mm, 0.5 mm pitch) |
| I2C Channels | 2 (SCL + SDA) |
| Max I2C Speed | 1 MHz |
| VCCA Range | 0.9 V – 3.6 V |
| VCCB Range | 0.9 V – 5.5 V |
| ESD Protection | 8 kV HBM |
| Direction Control | Automatic (no DIR pin) |
| PCB Size | 30 mm × 19 mm |
| PCB Layers | 2 (FR4, 1.6 mm) |
| Corner Radius | 3 mm |
| LCSC Part No. | C840107 |

---

## Schematic Overview

The board routes Side A and Side B I2C signals through U1 with independent power supplies on each side. The EN pin is permanently pulled high via R5, keeping the translator always enabled.

```
  [J1 — Connector A]           [J2 — Connector B]
  Pin 1: VCCA                  Pin 1: VCCB
  Pin 2: GND                   Pin 2: GND
  Pin 3: SCL_A                 Pin 3: SCL_B
  Pin 4: SDA_A                 Pin 4: SDA_B
         |                            |
         +----[U1: TCA9406DCUR]-------+
                  EN ← R5 (10k) ← VCCA
```

### Connectors

| Ref | Type | Pinout |
|---|---|---|
| J1 | 1×4 2.54 mm pin header | VCCA, GND, SCL_A, SDA_A |
| J2 | 1×4 2.54 mm pin header | VCCB, GND, SCL_B, SDA_B |

### Optional Pull-Up Resistors

Four solder jumpers select whether the onboard 10 kΩ pull-ups are connected. All are **open (disabled) by default**.

| Jumper | Signal | Resistor |
|---|---|---|
| JP1 | SDA_A | R1 (10 kΩ) to VCCA |
| JP2 | SCL_A | R2 (10 kΩ) to VCCA |
| JP3 | SDA_B | R3 (10 kΩ) to VCCB |
| JP4 | SCL_B | R4 (10 kΩ) to VCCB |

Close a jumper by bridging the two pads with solder. Leave open if your host system or target device already provides pull-ups.

### Decoupling

| Ref | Value | Location |
|---|---|---|
| C1, C3 | 1 µF 0603 | VCCA / VCCB bulk decoupling |
| C2, C4 | 100 nF 0603 | VCCA / VCCB HF decoupling |

### Test Points

| Ref | Signal |
|---|---|
| TP1 | GND |
| TP2 | VCCA |
| TP3 | VCCB |
| TP4 | SDA_A |
| TP5 | SDA_B |
| TP6 | SCL_A |
| TP7 | SCL_B |

All test points use 1.5 mm SMD pads.

---

## PCB Details

| Parameter | Value |
|---|---|
| Stackup | 2-layer FR4 |
| Board thickness | 1.6 mm |
| Copper thickness | 35 µm (1 oz) |
| Min clearance | 0.1016 mm |
| Min track width | 0.2 mm |
| Via (OD / drill) | 0.6 mm / 0.4 mm |
| Ground pour | Both layers |
| Teardrops | Enabled (PTH pads, vias, SMD pads) |
| Corner style | Rounded, r = 3 mm |

All resistors and capacitors use **0603 hand-solder footprints** with enlarged pads for ease of assembly.

---

## Bill of Materials (Abridged)

| Ref | Qty | Value / Part | Package | LCSC |
|---|---|---|---|---|
| U1 | 1 | TCA9406DCUR | VSSOP-8 | C840107 |
| R1–R5 | 5 | 10 kΩ | 0603 | — |
| C1, C3 | 2 | 1 µF | 0603 | — |
| C2, C4 | 2 | 100 nF | 0603 | — |
| J1, J2 | 2 | 1×4 2.54 mm pin header | Through-hole | — |
| JP1–JP4 | 4 | Solder jumper (2-pad, open) | 1.3 mm pitch | — |
| TP1–TP7 | 7 | Test point | SMD 1.5 mm pad | — |

---

## Usage Notes

- Supply **VCCA** and **VCCB** independently to match the voltage levels of the two I2C domains.
- The translator is **bidirectional and auto-sensing** — no firmware or direction control is needed.
- R5 (10 kΩ) pulls EN high from VCCA permanently. To disable the translator, cut the R5 pad or replace with a header and drive EN low externally.
- Close JP1–JP4 only if external pull-ups are absent on that signal line. Double pull-ups will lower the effective resistance and may violate I2C specifications for your bus capacitance.

---

## Repository

| File | Description |
|---|---|
| `TCA9406_Breakout_RevA.kicad_sch` | Schematic |
| `TCA9406_Breakout_RevA.kicad_pcb` | PCB layout |
| `TCA9406_Breakout_RevA.kicad_pro` | KiCAD project file |
| `TCA9406_Breakout_RevA-backups/` | KiCAD auto-backups |

Designed with **KiCAD 9.0**. Datasheet: [TCA9406 (ti.com)](https://www.ti.com/lit/ds/symlink/tca9406.pdf)
