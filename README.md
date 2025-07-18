# Akai LPD8 MK2 SysEx Implementation

## SysEx Message Structure


```

F0  47  <DevID>  4C  <Cmd>  <Sub‑ID/Payload…>  F7
│   │       │    │      │             │
│   │       │    │      │             → SysEx end
│   │       │    │      → command‑specific data
│   │       │    → Product ID (4C = LPD8 mk2)
│   │       → Device ID (00–7F or 7F = “All”)
│   → Akai manufacturer ID
→ SysEx start


```


---

## Pad Assignments & Addresses



| Pad # | SysEx Index | ID Byte | Note Number | Default Channel |
| --- | --- | --- | --- | --- |
| 1 | 0x06 (6) | 0x09 | 36 | Ch 1 (0x00) |
| 2 | 0x07 (7) | 0x09 | 37 | Ch 1 (0x00) |
| 3 | 0x08 (8) | 0x09 | 38 | Ch 1 (0x00) |
| 4 | 0x09 (9) | 0x09 | 39 | Ch 1 (0x00) |
| 5 | 0x0A (10) | 0x09 | 40 | Ch 1 (0x00) |
| 6 | 0x0B (11) | 0x09 | 41 | Ch 1 (0x00) |
| 7 | 0x0C (12) | 0x09 | 42 | Ch 1 (0x00) |
| 8 | 0x0D (13) | 0x09 | 43 | Ch 1 (0x00) |



---

## All‑8‑Pad Color Examples

### 1) Static White (all pads)


```

F0 47 7F 4C 06 00 30
  00 7F 00 7F 00 7F   ← Pad 1 (white)
  00 7F 00 7F 00 7F   ← Pad 2
  00 7F 00 7F 00 7F   ← Pad 3
  00 7F 00 7F 00 7F   ← Pad 4
  00 7F 00 7F 00 7F   ← Pad 5
  00 7F 00 7F 00 7F   ← Pad 6
  00 7F 00 7F 00 7F   ← Pad 7
  00 7F 00 7F 00 7F   ← Pad 8
F7


```
Each `(127,127,127)` splits into `[00 7F] [00 7F] [00 7F]`.



---

### 2) Rainbow Across All Pads



| Pad | Color (R,G,B) | Hex pairs (hi,lo) |
| --- | --- | --- |
| 1 | (127,  0,  0) | [00 7F] [00 00] [00 00] |
| 2 | (127, 63,  0) | [00 7F] [00 3F] [00 00] |
| 3 | (127,127,  0) | [00 7F] [00 7F] [00 00] |
| 4 | (  0,127,  0) | [00 00] [00 7F] [00 00] |
| 5 | (  0, 63,127) | [00 00] [00 3F] [00 7F] |
| 6 | (  0,  0,127) | [00 00] [00 00] [00 7F] |
| 7 | ( 63,  0,127) | [00 3F] [00 00] [00 7F] |
| 8 | (127,  0, 63) | [00 7F] [00 00] [00 3F] |

Full message:


```

F0 47 7F 4C 06 00 30
  00 7F 00 00 00 00   ← Pad 1: red
  00 7F 00 3F 00 00   ← Pad 2: orange
  00 7F 00 7F 00 00   ← Pad 3: yellow
  00 00 00 7F 00 00   ← Pad 4: green
  00 00 00 3F 00 7F   ← Pad 5: teal
  00 00 00 00 00 7F   ← Pad 6: blue
  00 3F 00 00 00 7F   ← Pad 7: violet
  00 7F 00 00 00 3F   ← Pad 8: magenta
F7


```


---

### 3) Alternate On/Off Pattern


```

F0 47 7F 4C 06 00 30
  00 7F 00 7F 00 7F   ← Pad 1: white
  00 00 00 00 00 00   ← Pad 2: off
  00 7F 00 7F 00 7F   ← Pad 3: white
  00 00 00 00 00 00   ← Pad 4: off
  00 7F 00 7F 00 7F   ← Pad 5: white
  00 00 00 00 00 00   ← Pad 6: off
  00 7F 00 7F 00 7F   ← Pad 7: white
  00 00 00 00 00 00   ← Pad 8: off
F7


```


| **Function** | **Cmd Byte** | **Sub‑ID / Header** | **Payload** | **Description** |
| --- | --- | --- | --- | --- |
| Get Program | 0x03 | 00 01 <Prog#> | — | Request the current Program (1 or 2) from the unit. |
| Program Data Response | 0x03 | 01 … | 128‑byte block starting with 0x29 (see “Program Data Format” below) | The unit replies with the full Program settings (pads, knobs assignments, colours, etc.). |
| Send Program | 0x01 | 01 29 … | 128‑byte block (0x29 + 15 entries of 8‑byte pad/knob configs + end) | Overwrites Program 1 on the unit with your custom pad‑knob mappings, ranges, modes, colours, etc. |
| **Pad LED Color Update** | **0x06** | **00 30** | **24 bytes: [R₁\_hi,R₁\_lo,G₁\_hi,G₁\_lo,B₁\_hi,B₁\_lo] … ×8 pads** | Set the 8 RGB pad LEDs. Each channel = 0–127, split into two 7‑bit bytes. |
| Set Global Channel | 0x01 | 01 29 … 0E 00 01 00 | … | Change the MIDI channel for all pads/knobs. 0E byte = channel (0–0F). |
| Pad Mode (Momentary/Toggle) | 0x01 | 01 29 … 0E 00 01 01 | … | Flip between momentary (0) and toggle (1) pad behaviour. |
| Pressure Message Mode | 0x01 | 01 29 … 0E 01 01 01 | … | 0x00=off, 0x01=Channel Pressure, 0x02=Polyphonic Pressure. |
| Full‑Level Velocity | 0x01 | 01 29 … 0E 02 00 01 | … | Enable (1) / disable (0) full‑level velocity output on pads. |
| **Reset to Default** | **0x01** | **01 29 01 00 00** | **… default 0x24/0x25/0x26/0x27/0x28/0x29 blocks …** | Restores all pad/knob mappings, ranges, colours, modes back to factory defaults. |

### Program Data Format (0x29 block)

Each pad/knob entry in the Send‑Program message uses this 8‑byte structure:


```

0x29
  <Index>        ; 00=K1, 01=K2, …, 06=Pad1, … 0D=Pad8
  <ID>           ; 0C=Knob, 09=Pad
  <Chan>         ; MIDI channel (0–0x0F)
  <PC/Note>      ; CC number for knobs, Note number for pads
  <Min> <Max>    ; Min/max value (0–127) for knobs
  <Flags>        ; bit‑flags: momentary/toggle, pressure on/off, full‑level, etc.


```

## Sources

1. [Akai LPD8 MK2 Mapping (VirtualDJ Forum)](https://www.virtualdj.com/forums/249938/Wishes_and_new_features/Mapping__Akai_LPD8_MK2.html) — with thanks to DJdad for the clues  
2. [MPC Forums Discussion](https://www.mpc-forums.com/viewtopic.php?f=37&t=215728)
