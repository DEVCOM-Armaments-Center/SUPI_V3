# sUPI Electrical Interfaces — Capacitors

**DEPARTMENT OF THE ARMY**
U.S. Army Combat Capabilities Development Command, Armaments Center
Picatinny Arsenal, New Jersey 07806-5000

| | |
| --- | --- |
| **Office Symbol** | FCDD-ACE-P |
| **Date** | 30 March 2026 |
| **Subject** | sUPI Electrical Interfaces Capacitors |
| **Applies to** | Payload board `100068610-001`, platform board `100069026-001` |

> **DISTRIBUTION STATEMENT A. APPROVED FOR PUBLIC RELEASE; DISTRIBUTION IS UNLIMITED.**

---

## Background

Note Murata's business statement regarding the use of their products for weapon systems:
<https://www.murata.com/en-us/support/militaryrestriction>

To reduce burden, the following have been identified as possible alternatives. Values, dielectrics,
voltage ratings, and package sizes are unchanged from the released BOMs.

---

## Payload — `100068610-001`

| Ref | Qty | Value | Description | Alternate MPN | Manufacturer |
| --- | --- | --- | --- | --- | --- |
| `C1`, `C6` | 2 | 4.7 µF 50 V | CAP CER 4.7UF 50V X7R 0805 | `C2012X7R1H475K125AC` | TDK |
| `C2` | 1 | 2.2 µF 35 V | CAP CER 2.2UF 35V X5R 0402 | `C1005X5R1V225K050BC` | TDK |
| `C3` | 1 | 10 µF 50 V | CAP CER 10UF 50V X5R 0805 | `CL21A106KBYQNNE` | Samsung |
| `C4`, `C7`, `C10`, `C11`, `C21`, `C22`, `C23`, `C24` | 8 | 0.1 µF 50 V | CAP CER 0.1UF 50V X7R 0402 | `CGA2B3X7R1H104M050BB` | TDK |
| `C5`, `C9` | 2 | 10 µF 6.3 V | CAP CER 10UF 6.3V X5R 0402 | `GMC04X5R106M6R3NT` | CAL-Chip Electronics |
| `C8` | 1 | 10000 pF 50 V | CAP CER 10000PF 50V X7R 0402 | `CGA2B3X7R1H103K050BB` | TDK |
| `C12`, `C13` | 2 | 10 pF 50 V | CAP CER 10PF 50V C0G/NP0 0402 | `04025U100F4T2A` | Kyocera AVX |
| `C14`, `C15`, `C16`, `C17`, `C18`, `C19` | 6 | 22 µF 25 V | CAP CER 22UF 25V X5R 0805 | `C2012X5R1V226M125AC` | TDK |

## Platform — `100069026-001`

| Ref | Qty | Value | Description | Alternate MPN | Manufacturer |
| --- | --- | --- | --- | --- | --- |
| `C1`, `C2`, `C6`, `C7`, `C13`, `C18`, `C23`, `C27`, `C28`, `C29`, `C30`, `C31`, `C32`, `C33` | 14 | 0.1 µF 50 V | CAP CER 0.1UF 50V X7R 0402 | `C1005X7R1H104K050BB` | TDK |
| `C3`, `C11`, `C12` | 3 | 10 µF 50 V | CAP CER 10UF 50V X5R 0805 | `C0805B106K050T` | Holy Stone Enterprise Co |
| `C4`, `C14` | 2 | 10000 pF 50 V | CAP CER 10000PF 50V X7R 0402 | `CC0402KRX7R9BB103` | YAGEO |
| `C5`, `C9` | 2 | 10 µF 6.3 V | CAP CER 10UF 6.3V X5R 0402 | `CL05A106MP5NUNC` | Samsung |
| `C8` | 1 | 4700 pF 50 V | CAP CER 4700PF 50V X7R 0402 | `C0402C472K5RACTU` | Kemet |
| `C10` | 1 | 0.33 µF 10 V | CAP CER 0.33UF 10V X5R 0402 | `CGA2B1X7S1C334M050BC` | TDK |
| `C24` | 1 | 4.7 µF 50 V | CAP CER 4.7UF 50V X7R 0805 | `CGA4J1X7R1H475K125AC` | TDK |

---

## Positions Not Covered

These BOM positions are **not** Murata parts as released and need no substitution:

- Payload `C25`, `C26`, `C27` — 22 µF 35 V, `C2012X5R1V226M125AC`, TDK
- Platform `C15`, `C16`, `C17`, `C19`, `C20`, `C21`, `C22` — 22 µF 35 V, `C2012X5R1V226M125AC`, TDK

Every other capacitor position on both boards is Murata as released and is addressed above. Note
that the released **platform** `C10` is a `GRT`-series part (automotive, soft-termination); the
alternate is an `X7S` dielectric rather than the original `X5R`. Confirm this is acceptable for the
application's temperature range.

---

**Respectfully submitted,**

Robert Barone Jr, PMP\
Special Project APO\
U.S. Army Combat Capabilities Development Command, Armaments Center\
Picatinny Arsenal, NJ 07806\
W: 520-684-1640

---

*Converted from `MFR_Capacitors.pdf`. Tables are verbatim from the source memo. The "Positions Not
Covered" section was added by cross-checking the source against the released BOMs
(`100068610-001_Rev2`, `100069026-001_Rev2`).*
