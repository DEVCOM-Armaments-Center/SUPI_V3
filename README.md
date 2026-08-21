# sUPI — Small Universal Payload Interface

**Technical Data Package, Version 3.3**

> **DISTRIBUTION STATEMENT A. APPROVED FOR PUBLIC RELEASE; DISTRIBUTION IS UNLIMITED.**

---

## Overview

sUPI is an interchangeable mechanical and electrical interface for attaching payloads to uncrewed
systems, primarily small UAS.

The interface is two mating halves, each with its own machined hardware and circuit board:

| Half | Mounts to | Board | Role |
| --- | --- | --- | --- |
| **Platform Side** | The vehicle | `100069026-001` | Gates and monitors payload power; switches USB; hosts the interface MCU |
| **Payload Side** | The payload | `100068610-001` | Regulates platform battery down to payload rails; carries the payload ID device |

Power, USB, and GPIO cross the interface on spring contacts, so the halves mate and separate without
a cable or locking connector. The platform side is the intelligent end: a detect mechanism lets it
identify an attached payload and gate the payload signals accordingly.

Start with [`sUPI_Platform_Brief.md`](sUPI_Platform_Brief.md) or
[`sUPI_Payload_Brief.md`](sUPI_Payload_Brief.md) depending on which side you are integrating.

---

## Electrical Interface

### Platform → Platform-Side Board

| Signal | Range |
| --- | --- |
| `VBATT` / `VBATT_RET` | 13 – 25.2 VDC (4S–6S pack) |
| `GPIO` | Typically PWM, ±5 VDC |
| `USB` | 0 – 3.3 VDC |

### Payload-Side Board → Payload

| Signal | Range |
| --- | --- |
| `VBATT` / `VBATT_RET` | 13 – 25.2 VDC, passed through |
| `V1` | Regulated **3.3 or 5 VDC** — set by solder bridge |
| `V2` | Regulated **5, 7.4, or 12 VDC** — set by solder bridge |
| `GPIO` | Typically PWM, ±5 VDC |
| `USB` | 0 – 3.3 VDC |

> **V1 and V2 are hardware-selected.** The rails are set by solder-bridge placement
> at build time. Verify the bridges match the payload before first power-on.

Both boards use solder pads for field wiring: 18–22 AWG for power, 24–30 AWG for signals.

### Board Markings

- **Platform pads:** `VBATT`, `GND`, `GPIO 0`, `GPIO 1`, `+USB−`, `AUX`
- **Platform jumpers:** `DETECT PAYLOAD`, `SELECT`, `FORCE ON`, `LO BATT CUTOFF`
- **Payload pads:** `VBATT`, `GND`, `V1`, `V1 OK`, `V2`, `V2 OK`, `USB DP`, `USB DM`, `GPIO 0`, `GPIO 1`
- **Payload selectors:** `V1SEL` (`3V3` / `5V`), `V2SEL` (`5V` / `7V4` / `12V`)

`FORCE ON` overrides the detect gate — confirm its state before flight.

### Functional Blocks

**Platform side** — 8-layer board.

| Function | Device |
| --- | --- |
| Interface MCU | STM32C011F6 (Arm Cortex-M0+) |
| Payload power switch | MPQ5069, 15 A |
| Current sense | ACS711, ±15 A bidirectional Hall-effect |
| Ideal-diode / reverse-polarity control | NCV68261 driving an N-channel MOSFET |
| Undervoltage supervisor | LTC2965 |
| Logic 3.3 V rail | NCP730 LDO, 150 mA |
| USB routing | TC7USB40MU dual SPDT, MAX20336 DPST |
| Transient protection | 26 V bidirectional TVS on `VBATT`, 3.3 V TVS on signals |

**Payload side** — 4-layer board.

| Function | Device |
| --- | --- |
| V1 / V2 regulation | MAX25254, dual 36 V in / 0.8–14 V out, 8 A synchronous buck |
| Payload ID / detect | DS2431 1-Wire EEPROM, 1 kbit |
| Input protection | NCV68261 + N-channel MOSFET, P-channel blocking FET |
| Logic 3.3 V rail | NCP730 LDO, 150 mA |
| Supply supervisor | MAX821, 2.78 V |
| USB routing | TC7USB40MU dual SPDT, MAX20336 DPST |
| Transient protection | 26 V unidirectional TVS on `VBATT`, 3.3 V TVS on signals |

The DS2431 is what the platform MCU reads to identify an attached payload — a payload built to this
interface inherits an identity device it does not have to implement itself.

### Mating Connection

A **9-contact, 0.100"-pitch right-angle spring-pin interface.** The payload side carries the spring
pins (Mill-Max 829-series); the platform side carries the mating targets (Mill-Max 399-series).

---

## Mechanical Interface

Both halves mount with **M3 × 0.5 mm thread, 8 mm long, black-oxide stainless steel Phillips
flat-head screws.**

| Half | Screws | Controlling drawing |
| --- | --- | --- |
| Platform side | 3X M3 | `19200_13122518` |
| Payload side | 4X M3 | `19200_13122514` |

Two accepted installation methods, either half:

1. **Direct mount** — drill and tap the sUPI hole pattern into the host structure.
2. **Adapter** — fabricate an adapter carrying the sUPI hole pattern on one face and mating to an
   existing interface on the other. Preferred where the host structure cannot be modified.

The payload-side pattern carries a `VEHICLE CG` reference callout — position the interface so the
vehicle center of gravity lands as close to that point as the installation allows.

### Part Tree

Assembly **13122513**, one each of:

| Part No. | Nomenclature | Material | Process / Finish |
| --- | --- | --- | --- |
| `13122514` | V-Mount Base | 6061-T6 per ASTM B209, or polymer powder bed fusion | Bead blast; hard anodize per MIL-A-8625F Type III Class 2, black, sealed |
| `13122515` | Short Button | 6061-T6 per ASTM B209 | Bead blast; hard anodize per MIL-A-8625F Type III Class 2, black, sealed |
| `13122516` | Small Button Cover | PA12 or PA6 | Polymer powder bed fusion (MJF, SLS, or SAF); depowder, media blast, optional black dye |
| `13122517` | Connector Cover | 6061-T6 per ASTM B209, or polymer powder bed fusion | Bead blast; hard anodize per MIL-A-8625F Type III Class 2, black, sealed |
| `13122518` | V-Mount | 6061-T6 per ASTM B209 | Mask/plug threaded holes, then bead blast and hard anodize per MIL-A-8625F Type III Class 2, black, sealed |

Threaded and clearance features:

- `13122514` — 4× M3 clearance, 90° countersunk; 2× M2 × 0.4 tapped
- `13122517` — 2× M2 × 0.4 clearance
- `13122518` — 3× M3 clearance, 90° countersunk; 3× M1.6 × 0.35 tapped, through

Dimensions are in inches; interpret per ANSI/ASME Y14.5M-2009. **The drawings are the authority** —
each detail sheet names a design model required to complete the product definition, and
undimensioned features are taken from that model. Do not machine from the PDFs alone.

---

## Before You Fabricate Boards

The three notes in `sUPI Electrical/` supersede the released BOMs. Read them first.

| Note | Direction |
| --- | --- |
| [`MFR_DNP.md`](sUPI%20Electrical/MFR_DNP.md) | Do not populate platform `U3`, `R1`–`R4`, `C2`, `JP1` — the undervoltage cutoff is still in development and can block power to the payload |
| [`MFR_Rework.md`](sUPI%20Electrical/MFR_Rework.md) | Remove platform `U11` and `U12` and bridge pin 2 to pin 4 on each footprint — a bare do-not-populate leaves the net open and does not implement the workaround |
| [`MFR_Capacitors.md`](sUPI%20Electrical/MFR_Capacitors.md) | Alternate capacitor part numbers for every Murata position on both boards |

With the do-not-populate and rework direction applied, the platform board has **no undervoltage
supervision and no supply supervisor**, and its detect/enable gate is strapped to a permanent
pass-through. If your application depends on brownout protection or supervised power sequencing,
treat that as an open engineering item.

---

## Firmware

`supi_firmware.hex` is the platform MCU image: 20,440 bytes, load address `0x08000000`, targeting the
STM32C011 (Arm Cortex-M0+). Intel HEX format.

---

## Revision History

See [`CHANGELOG.md`](CHANGELOG.md).

v3.3 is a manufacturability and fit release — it removes special tooling from the machining plan and
takes slop out of the mating stack. No electrical interface changes.

**v3.3 hardware is physically marked.** Check the engraved version number before mating
mixed-vintage halves: earlier parts have looser fit, and earlier `13122515` buttons have the taller
profile that was prone to bending.

---

## Package Contents

```
.
├── CHANGELOG.md                     Design change history, v3.1 → v3.3
├── sUPI_Platform_Brief.md           Platform-side integration brief
├── sUPI_Payload_Brief.md            Payload-side integration brief
├── assets/                          Figures for the briefs and changelog
│
├── sUPI Mechanicals/
│   ├── 19200_13122513_v3-3.pdf      Assembly drawing
│   ├── 19200_1312251[4-8]_v3-3.pdf  Detail drawings
│   └── 19200_*_v3-3.stp             Solid models (STEP)
│
├── sUPI Electrical/
│   ├── MFR_DNP.md                   Do-not-populate direction
│   ├── MFR_Rework.md                Platform rework work instructions
│   ├── MFR_Capacitors.md            Capacitor alternate sourcing
│   ├── Platform/
│   │   ├── 100046138_Rev2.pdf       Schematic
│   │   ├── 100069026-001_Rev1_FAB_DWG.pdf
│   │   ├── *.gbr                    Gerbers — 8 copper layers
│   │   ├── *.drl, *-DRL.rpt         Drill files and report
│   │   ├── *.d356                   Bare-board test netlist
│   │   └── *_Rev2__platform_VAL_bom.xlsx
│   └── Payload/
│       ├── 100046137_Rev2.pdf       Schematic
│       ├── 100068610-001_Rev1_FAB_DWG.pdf
│       ├── *.gbr                    Gerbers — 4 copper layers
│       ├── *.drl, *-DRL.rpt         Drill files and report
│       ├── *.d356                   Bare-board test netlist
│       └── *_Rev2_payload_VAL_bom.xlsx
│
└── sUPI Firmware/
    └── supi_firmware.hex            Platform MCU image
```

### Where to Start

| You are | Read, in order |
| --- | --- |
| **Platform integrator** | Platform brief → assembly drawing → platform schematic |
| **Payload integrator** | Payload brief → assembly drawing → payload schematic |
| **Fabricating boards** | `MFR_*.md` notes → BOM → Gerbers, drill, fab drawing |
| **Machining parts** | Detail drawing + its matching STEP model |
| **Bringing up firmware** | Platform schematic, MCU section → `supi_firmware.hex` |

---

## Conventions

- **Mechanical part numbers** are 8-digit design-activity numbers, prefixed in filenames with CAGE
  code `19200` and suffixed with the TDP version (`_v3-3`).
- **Electrical part numbers** are 9-digit with a `-001` dash number and an explicit `_Rev`.
- **Dimensions** are in inches unless otherwise specified; tolerances per ANSI/ASME Y14.5M-2009.
  Untoleranced features carry a 0.005 profile tolerance to datums A-B-C.
- **Mechanical files:** `.pdf` released drawing, `.stp` neutral solid model.
- **Electrical files:** `.pdf` schematic and fab drawing, `.gbr` Gerber layer, `.drl` drill, `.d356`
  bare-board test netlist, `.xlsx` BOM.
- **Packaging and marking** per MIL-STD-2073-1 Method 10 and MIL-STD-130.
