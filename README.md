# AT49F512 Flash Programmer & Hardware Analysis Lab

An interactive, browser-based engineering lab for learning the **Atmel AT49F512** 512Kbit / 64Kbyte 5V parallel NOR Flash memory.

This project explains the AT49F512 from the silicon level to the practical programmer level:

```text
Flash cell
→ floating gate
→ memory array
→ address decoder
→ data path
→ control logic
→ parallel bus
→ read/write/erase commands
→ hardware programmer concept
→ binary dump
→ hex inspection
→ byte modification
→ erase
→ program
→ verify
```

The repository includes an interactive HTML lab with a Flash simulator, command sequence player, pinout explorer, bus-state explorer, hex inspector, bit-rule checker, timing reference, safety checklist, and hardware workflow.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Repository Contents](#repository-contents)
- [Quick Start](#quick-start)
- [Running in VS Code](#running-in-vs-code)
- [Running with Python](#running-with-python)
- [Deploying with GitHub Pages](#deploying-with-github-pages)
- [Interactive Features](#interactive-features)
- [Device Summary](#device-summary)
- [Datasheet Source](#datasheet-source)
- [AT49F512 Description](#at49f512-description)
- [Memory Organization](#memory-organization)
- [Memory Map](#memory-map)
- [Pin Configuration](#pin-configuration)
- [Operating Modes](#operating-modes)
- [Read Operation](#read-operation)
- [Erase Operation](#erase-operation)
- [Byte Programming](#byte-programming)
- [Command Definition Table](#command-definition-table)
- [Product Identification](#product-identification)
- [Boot Block and Boot Block Lockout](#boot-block-and-boot-block-lockout)
- [DATA Polling](#data-polling)
- [Toggle Bit](#toggle-bit)
- [Hardware Data Protection](#hardware-data-protection)
- [DC Characteristics](#dc-characteristics)
- [AC Read Characteristics](#ac-read-characteristics)
- [AC Byte Load / Program Cycle Characteristics](#ac-byte-load--program-cycle-characteristics)
- [Data Polling and Toggle Bit Timing](#data-polling-and-toggle-bit-timing)
- [Absolute Maximum Ratings](#absolute-maximum-ratings)
- [Package Information](#package-information)
- [Ordering Information](#ordering-information)
- [Hardware Programmer Design Notes](#hardware-programmer-design-notes)
- [Safe Engineering Workflow](#safe-engineering-workflow)
- [Example Experiments](#example-experiments)
- [Troubleshooting](#troubleshooting)
- [Source Discipline](#source-discipline)
- [Disclaimer](#disclaimer)
- [License](#license)

---

## Project Overview

The AT49F512 is a classic 5V-only parallel Flash memory. It is a good device for learning how parallel NOR Flash works because it has:

- A simple 16-bit address bus
- An 8-bit bidirectional data bus
- Classic `CE`, `OE`, and `WE` control signals
- Software command sequences for erase, program, and identification
- DATA polling and toggle-bit status detection
- An optional boot block with lockout

This project is not only a software simulator. It is intended as a learning companion for building a real hardware programmer.

The interactive lab helps you understand:

- How to identify the chip
- How to read the chip
- How to dump the full 64KB contents
- How to inspect the binary image
- How to modify bytes safely
- How to erase the chip
- How to program modified data
- How to verify the programmed result
- How to avoid damaging the device

---

## Repository Contents

Minimal repository layout:

```text
AT49F512-lab/
├── README.md
└── AT49F512_interactive.html
```

Recommended layout for a larger project:

```text
AT49F512-lab/
├── README.md
├── LICENSE
├── index.html
├── docs/
│   ├── notes.md
│   ├── pinout.md
│   ├── timing.md
│   └── workflow.md
├── hardware/
│   ├── schematic.pdf
│   ├── schematic.svg
│   └── wiring.md
├── firmware/
│   ├── programmer/
│   └── examples/
└── scripts/
    ├── dump.py
    ├── verify.py
    └── program.py
```

If you want to use GitHub Pages, rename:

```text
AT49F512_interactive.html
```

to:

```text
index.html
```

---

## Quick Start

1. Clone or download the repository.

```bash
git clone https://github.com/USERNAME/AT49F512-lab.git
cd AT49F512-lab
```

2. Open the HTML file in a browser.

```text
AT49F512_interactive.html
```

No build step is required.

No external JavaScript libraries are required.

The page is self-contained.

---

## Running in VS Code

### Method 1: Direct open

1. Open the project folder in VS Code.
2. Open:

```text
AT49F512_interactive.html
```

3. Right-click in the editor.
4. Choose:

```text
Reveal in File Explorer
```

or:

```text
Reveal in Finder
```

5. Double-click the HTML file.

It should open in your default browser.

---

### Method 2: Live Server extension

This is the recommended method.

1. Install VS Code.
2. Install the extension:

```text
Live Server
```

by Ritwick Dey.

3. Open the project folder in VS Code.
4. Open:

```text
AT49F512_interactive.html
```

5. Click:

```text
Go Live
```

in the bottom-right corner.

Or right-click inside the editor and choose:

```text
Open with Live Server
```

The browser will open something like:

```text
http://127.0.0.1:5500/AT49F512_interactive.html
```

Live Server also reloads the page automatically when you save changes.

---

## Running with Python

If Python is installed, open a terminal in the repository folder and run:

```bash
python -m http.server 8000
```

or:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/AT49F512_interactive.html
```

If the file is named `index.html`, open:

```text
http://localhost:8000/
```

Stop the server with:

```text
Ctrl + C
```

---

## Deploying with GitHub Pages

1. Push the repository to GitHub.
2. Rename the interactive file to:

```text
index.html
```

3. Go to:

```text
Repository → Settings → Pages
```

4. Under `Build and deployment`, choose:

```text
Deploy from a branch
```

5. Select:

```text
main branch
/ root
```

6. Save.

The page should become available at:

```text
https://USERNAME.github.io/REPOSITORY_NAME/
```

---

## Interactive Features

### Flash Simulator

The built-in simulator models the main behavior of the AT49F512:

- Read byte
- Program byte
- Chip erase
- Software product ID entry
- Software product ID exit
- Manufacturer/device ID read
- Boot block lockout detection
- Boot block lockout enable
- Hex dump download
- Binary file loading for inspection

The simulator enforces the fundamental NOR Flash rule:

```text
Programming can only change bits from 1 → 0.
Erasing is required to change bits from 0 → 1.
```

---

### Command Sequence Player

The command sequence player steps through the AT49F512 bus cycles for:

- Read
- Byte Program
- Chip Erase
- Product ID Entry
- Product ID Exit
- Boot Block Lockout

Each cycle shows:

- Address
- Data
- CE state
- OE state
- WE state

---

### Pinout Explorer

The pinout explorer shows the 32T TSOP / 32V VSOP pin arrangement.

Pin groups:

- Address pins
- Data pins
- Control pins
- Power pins
- No-connect pins

Each pin shows behavior during:

- READ
- PROGRAM / COMMAND
- ERASE / command sequence

---

### Bus State Explorer

Interactive selector for:

- Read mode
- Program/command mode
- Standby/write inhibit
- Output disable
- Product ID read

Shows:

- CE
- OE
- WE
- Which device drives the data bus
- Bus safety notes

---

### Hex Inspector

Inspect the simulated Flash image:

- Address offsets
- Hex bytes
- ASCII representation
- Byte selection
- Direct link to simulator address

---

### Bit Rule Checker

Enter:

```text
Current byte
Desired byte
```

The tool tells you whether the desired value can be programmed directly, or whether erase is required.

Example:

```text
Current: 0xFF
Desired: 0x42
```

Result:

```text
Possible without erase, because all changed bits are 1 → 0.
```

Example:

```text
Current: 0x42
Desired: 0xFF
```

Result:

```text
Erase required, because some bits require 0 → 1.
```

---

## Device Summary

| Item | Value |
|---|---|
| Device | AT49F512 |
| Type | 5V-only in-system programmable/erasable Flash |
| Density | 512Kbit |
| Organization | 65,536 words × 8 bits |
| Total memory | 65,536 bytes |
| Memory size in bytes | 64Kbytes |
| Address range | `0x0000` to `0xFFFF` |
| Address pins | A0–A15 |
| Data pins | I/O0–I/O7 |
| Supply voltage | 5V ±10% |
| Read access time | 55 ns for `-55` parts |
| Byte program time | 10 µs typical, 50 µs maximum |
| Chip erase time | 10 seconds maximum |
| Manufacturer code | `0x1F` |
| Device code | `0x03` |
| Boot block | 8K bytes |
| Boot block address range | `0x0000` to `0x1FFF` |
| Write cycles | 10,000 typical |
| Packages | 32J PLCC, 32T TSOP, 32V VSOP |

---

## Datasheet Source

This project is based on the AT49F512 datasheet:

```text
AT49F512
512K(64K x 8) 5-volt Only Flash Memory
1027F–FLASH–3/05
```

Facts in this README that come from the datasheet are marked where useful as:

```text
[DATASHEET]
```

Practical engineering suggestions are marked as:

```text
[DESIGN RECOMMENDATION]
```

General electronics knowledge not explicitly stated in the datasheet is marked as:

```text
[ENGINEERING KNOWLEDGE]
```

Reasonable conclusions derived from datasheet behavior are marked as:

```text
[INFERENCE]
```

---

## AT49F512 Description

The AT49F512 is a 5-volt-only in-system programmable and erasable Flash memory.

Its 512K of memory is organized as:

```text
65,536 words by 8 bits
```

This equals:

```text
65,536 bytes
```

or:

```text
64Kbytes
```

The device does not require high input voltages for programming. Five-volt-only commands determine the read and programming operation of the device.

Reading data from the device is similar to reading from an EPROM.

Reprogramming is performed by:

1. Erasing the memory array.
2. Programming on a byte-by-byte basis.

The typical byte programming time is:

```text
10 µs
```

The maximum byte programming time is:

```text
50 µs
```

The end of a program cycle can optionally be detected using the DATA polling feature.

The typical number of program and erase cycles is in excess of:

```text
10,000 cycles
```

The optional 8K byte boot block section includes a reprogramming write lockout feature to provide data integrity.

When the boot block lockout feature is enabled, the boot sector is permanently protected from being reprogrammed.

---

## Memory Organization

The AT49F512 is organized as:

```text
65,536 addressable byte locations
```

Each location contains:

```text
8 bits
```

Therefore:

```text
65,536 × 8 = 524,288 bits = 512Kbit
```

This is why the device is called:

```text
512K
```

but it is important to understand:

```text
512Kbit ≠ 512Kbyte
```

The AT49F512 is:

```text
512Kbit = 64Kbyte
```

---

## Memory Map

The complete user address range is:

```text
0x0000 to 0xFFFF
```

The standard boot block is located in the lower address range:

```text
0x0000 to 0x1FFF
```

This is 8Kbytes.

The remaining memory area is:

```text
0x2000 to 0xFFFF
```

Memory map:

```text
+-----------------------------+ 0xFFFF
|                             |
| Main memory area            |
| 0x2000 to 0xFFFF            |
|                             |
+-----------------------------+ 0x2000
|                             |
| Boot block                  |
| 8K bytes                    |
| 0x0000 to 0x1FFF            |
|                             |
+-----------------------------+ 0x0000
```

If the boot block feature is used, the main memory area is described as 56K bytes.

The standard ordering uses the boot block in the lower address range. Devices with the boot block in the higher address range were special-order parts.

---

## Pin Configuration

### Pin Functions

| Pin Name | Function |
|---|---|
| A0–A15 | Addresses |
| CE | Chip Enable |
| OE | Output Enable |
| WE | Write Enable |
| I/O0–I/O7 | Data Inputs/Outputs |
| NC | No Connect |

---

### 32T TSOP / 32V VSOP Pinout

The following table is for the 32-lead TSOP Type 1 package and the 32-lead VSOP package.

| Pin | Name | Function |
|---:|---|---|
| 1 | A11 | Address input |
| 2 | A9 | Address input |
| 3 | A8 | Address input |
| 4 | A13 | Address input |
| 5 | A14 | Address input |
| 6 | NC | No connect |
| 7 | WE | Write enable |
| 8 | VCC | Power supply |
| 9 | NC | No connect |
| 10 | NC | No connect |
| 11 | A15 | Address input |
| 12 | A12 | Address input |
| 13 | A7 | Address input |
| 14 | A6 | Address input |
| 15 | A5 | Address input |
| 16 | A4 | Address input |
| 17 | A3 | Address input |
| 18 | A2 | Address input |
| 19 | A1 | Address input |
| 20 | A0 | Address input |
| 21 | I/O0 | Data bit 0 |
| 22 | I/O1 | Data bit 1 |
| 23 | I/O2 | Data bit 2 |
| 24 | GND | Ground |
| 25 | I/O3 | Data bit 3 |
| 26 | I/O4 | Data bit 4 |
| 27 | I/O5 | Data bit 5 |
| 28 | I/O6 | Data bit 6, toggle bit |
| 29 | I/O7 | Data bit 7, DATA polling |
| 30 | CE | Chip enable |
| 31 | A10 | Address input |
| 32 | OE | Output enable |

---

### 32J PLCC Pinout Caution

The 32J PLCC pinout is shown in the original datasheet drawing.

If you are working with a physical PLCC device:

1. Confirm the package marking.
2. Confirm pin 1 orientation.
3. Compare against the original datasheet figure.
4. Do not assume pinout from memory.

Incorrect pinout can damage the chip.

[DESIGN RECOMMENDATION]

---

### Pin 1 Identification

For TSOP and VSOP packages, the package drawing shows a Pin 1 identifier.

For PLCC packages, the package drawing also references a pin no. 1 identifier.

Look for:

```text
Dot
Beveled corner
Cut corner
Molded notch
Printed marker
```

[ENGINEERING KNOWLEDGE]

---

## Operating Modes

The AT49F512 has several basic operating modes.

| Mode | CE | OE | WE | Address | I/O |
|---|---|---|---|---|---|
| Read | VIL | VIL | VIH | Address | DOUT |
| Program | VIL | VIH | VIL | Address | DIN |
| Standby / Write Inhibit | VIH | X | X | X | High-Z |
| Program Inhibit | X | X | VIH | — | — |
| Program Inhibit | X | VIL | X | — | — |
| Output Disable | X | VIH | X | — | High-Z |

Important:

```text
Outputs are high impedance whenever CE or OE is high.
```

This dual-line control helps prevent bus contention.

---

## Read Operation

The AT49F512 is accessed like an EPROM.

Read condition:

```text
CE = LOW
OE = LOW
WE = HIGH
```

When these conditions are true:

```text
The data stored at the selected address is asserted on I/O0-I/O7.
```

Address selection:

```text
A0-A15 select one of 65,536 byte locations.
```

Outputs:

```text
I/O0-I/O7 drive the stored byte.
```

If CE or OE is high:

```text
Outputs are high impedance.
```

Example read:

```text
Address = 0x1234
CE = LOW
OE = LOW
WE = HIGH

After access time, I/O0-I/O7 contain the byte stored at 0x1234.
```

For AT49F512-55:

```text
tACC max = 55 ns
tCE max = 55 ns
tOE max = 30 ns
```

---

## Erase Operation

Before a byte can be reprogrammed, the relevant memory array must be erased.

The erased state of memory bits is:

```text
Logical 1
```

Therefore, an erased byte should usually read:

```text
0xFF
```

The entire device can be erased at one time using a 6-byte software code.

Chip erase command sequence:

| Cycle | Address | Data |
|---:|---:|---:|
| 1 | `0x5555` | `0xAA` |
| 2 | `0x2AAA` | `0x55` |
| 3 | `0x5555` | `0x80` |
| 4 | `0x5555` | `0xAA` |
| 5 | `0x2AAA` | `0x55` |
| 6 | `0x5555` | `0x10` |

After chip erase has been initiated:

```text
The device internally times the erase operation.
No external clocks are required.
```

Maximum chip erase time:

```text
tEC = 10 seconds
```

If boot block lockout has been enabled:

```text
Data in the boot sector will not be erased.
```

---

## Byte Programming

Once the memory array is erased, the device is programmed to logical `0` on a byte-by-byte basis.

Important rule:

```text
A data 0 cannot be programmed back to a 1.
Only erase operations can convert 0s to 1s.
```

Programming uses the internal device command register and is a 4 bus cycle operation.

Byte program command sequence:

| Cycle | Address | Data |
|---:|---:|---:|
| 1 | `0x5555` | `0xAA` |
| 2 | `0x2AAA` | `0x55` |
| 3 | `0x5555` | `0xA0` |
| 4 | Target address | Target data |

The device automatically generates the required internal program pulses.

Program cycle latching:

```text
Addresses are latched on the falling edge of WE or CE, whichever occurs last.
Data is latched on the rising edge of WE or CE, whichever occurs first.
```

Programming completes after:

```text
tBP
```

Typical:

```text
10 µs
```

Maximum:

```text
50 µs
```

The DATA polling feature may also be used to detect the end of a program cycle.

---

## Command Definition Table

| Command | Bus Cycles | Sequence |
|---|---:|---|
| Read | 1 | `Addr → DOUT` |
| Chip Erase | 6 | `5555:AA`, `2AAA:55`, `5555:80`, `5555:AA`, `2AAA:55`, `5555:10` |
| Byte Program | 4 | `5555:AA`, `2AAA:55`, `5555:A0`, `Addr:DIN` |
| Boot Block Lockout | 6 | `5555:AA`, `2AAA:55`, `5555:80`, `5555:AA`, `2AAA:55`, `5555:40` |
| Product ID Entry | 3 | `5555:AA`, `2AAA:55`, `5555:90` |
| Product ID Exit | 3 | `5555:AA`, `2AAA:55`, `5555:F0` |
| Product ID Exit alternate | 1 | `XXXX:F0` |

Notes:

```text
Addresses are hexadecimal.
Data values are hexadecimal.
Either Product ID exit command can be used.
```

---

## Product Identification

The product identification mode identifies the device and manufacturer.

Manufacturer code:

```text
0x1F
```

Device code:

```text
0x03
```

---

### Software Product Identification Entry

| Cycle | Address | Data |
|---:|---:|---:|
| 1 | `0x5555` | `0xAA` |
| 2 | `0x2AAA` | `0x55` |
| 3 | `0x5555` | `0x90` |

After entry:

| Address | Read Value | Meaning |
|---|---:|---|
| `0x0000` | `0x1F` | Manufacturer code |
| `0x0001` | `0x03` | Device code |
| `0x0002` | `0x00` or `0x01` | Boot block lockout status |

For software product identification:

```text
Manufacturer Code is read when A0 = VIL.
Device Code is read when A0 = VIH.
```

The device does not remain in identification mode if powered down.

---

### Software Product Identification Exit

Method 1:

| Cycle | Address | Data |
|---:|---:|---:|
| 1 | `0x5555` | `0xAA` |
| 2 | `0x2AAA` | `0x55` |
| 3 | `0x5555` | `0xF0` |

Method 2:

| Cycle | Address | Data |
|---:|---:|---:|
| 1 | Any address | `0xF0` |

After exit, the device returns to standard operation.

---

### Hardware Product Identification

The datasheet also describes hardware product identification using:

```text
A9 = VH
VH = 12.0V ± 0.5V
```

This mode is intended for external programmers.

Because this involves special voltage conditions, the safe learning approach is:

```text
Use software product identification only.
```

[DESIGN RECOMMENDATION]

---

## Boot Block and Boot Block Lockout

The device has one designated block with a programming lockout feature.

Boot block size:

```text
8K bytes
```

Standard boot block address range:

```text
0x0000 to 0x1FFF
```

The boot block can contain secure code used to bring up the system.

Enabling the lockout feature allows the boot code to remain in the device while data in the rest of the device is updated.

This feature is optional.

---

### Boot Block Lockout Effect

Once the feature is enabled:

```text
Data in the boot block can no longer be erased or programmed.
```

Data in the main memory block can still be changed through the regular programming method.

---

### Boot Block Lockout Command

| Cycle | Address | Data |
|---:|---:|---:|
| 1 | `0x5555` | `0xAA` |
| 2 | `0x2AAA` | `0x55` |
| 3 | `0x5555` | `0x80` |
| 4 | `0x5555` | `0xAA` |
| 5 | `0x2AAA` | `0x55` |
| 6 | `0x5555` | `0x40` |

The boot block lockout algorithm also shows:

```text
Pause 1 second
```

after the final command.

Warning:

```text
On real hardware, boot block lockout should be treated as permanent.
```

---

### Boot Block Lockout Detection

To detect whether the boot block is locked:

1. Enter software product identification mode.
2. Read address:

```text
0x0002
```

3. Check I/O0.

| I/O0 | Meaning |
|---|---|
| 0 | Boot block can be programmed |
| 1 | Boot block lockout has been activated |

After checking, exit software product identification mode.

---

## DATA Polling

The AT49F512 features DATA polling to indicate the end of a program cycle.

DATA polling uses:

```text
I/O7
```

During a program cycle:

```text
An attempted read of the last byte loaded will result in the complement of the loaded data on I/O7.
```

Once the program cycle is complete:

```text
True data is valid on all outputs.
```

DATA polling may begin at any time during the program cycle.

Example:

```text
Programming data = 0x42
0x42 = 01000010 binary
I/O7 true value = 0
During programming, I/O7 complement = 1
After completion, I/O7 = 0
```

---

## Toggle Bit

The AT49F512 also provides a toggle-bit method for determining the end of a program or erase cycle.

Toggle bit uses:

```text
I/O6
```

During a program or erase operation:

```text
Successive read attempts will show I/O6 toggling between 0 and 1.
```

When the cycle completes:

```text
I/O6 stops toggling and valid data is read.
```

Toggle-bit polling may begin at any time during a program cycle.

Notes from the datasheet:

```text
Toggling either OE or CE, or both, will operate the toggle bit.
The beginning and ending state of I/O6 will vary.
Any address location may be used, but the address should not vary.
```

---

## Hardware Data Protection

The AT49F512 includes hardware features that protect against inadvertent programming.

### VCC Sense

If VCC is below approximately:

```text
3.8V typical
```

then the program function is inhibited.

---

### Program Inhibit

Program cycles are inhibited by holding any one of the following:

```text
OE low
CE high
WE high
```

---

### Noise Filter

Pulses shorter than approximately:

```text
15 ns typical
```

on WE or CE will not initiate a program cycle.

---

## DC Characteristics

### DC and AC Operating Range

For AT49F512-55:

| Parameter | Value |
|---|---|
| Industrial operating temperature | -40°C to +85°C |
| VCC power supply | 5V ±10% |

---

### DC Characteristics

| Symbol | Parameter | Min | Max | Units |
|---|---|---:|---:|---|
| ILI | Input load current | — | 10 | µA |
| ILO | Output leakage current | — | 10 | µA |
| ISB1 | VCC standby current CMOS, commercial | — | 100 | µA |
| ISB1 | VCC standby current CMOS, industrial | — | 300 | µA |
| ISB2 | VCC standby current TTL | — | 3 | mA |
| ICC | VCC active current, commercial | — | 30 | mA |
| ICC | VCC active current, industrial | — | 40 | mA |
| VIL | Input low voltage | — | 0.8 | V |
| VIH | Input high voltage | 2.0 | — | V |
| VOL | Output low voltage | — | 0.45 | V |
| VOH1 | Output high voltage | 2.4 | — | V |
| VOH2 | Output high voltage CMOS | 4.2 | — | V |

Conditions for some parameters:

```text
VOL measured at IOL = 2.1 mA
VOH1 measured at IOH = -400 µA
VOH2 measured at IOH = -100 µA, VCC = 4.5V
```

---

### Pin Capacitance

| Symbol | Typical | Max | Units | Conditions |
|---|---:|---:|---|---|
| CIN | 4 | 6 | pF | VIN = 0V |
| COUT | 8 | 12 | pF | VOUT = 0V |

---

## AC Read Characteristics

For AT49F512-55:

| Symbol | Parameter | Min | Max | Units |
|---|---|---:|---:|---|
| tACC | Address to output delay | — | 55 | ns |
| tCE | CE to output delay | — | 55 | ns |
| tOE | OE to output delay | — | 30 | ns |
| tDF | CE or OE to output float | 0 | 25 | ns |
| tOH | Output hold from OE, CE, or address | 0 | — | ns |

Notes:

```text
tDF is specified from OE or CE, whichever occurs first.
Some parameters are characterized and not 100% tested.
```

---

## AC Byte Load / Program Cycle Characteristics

### AC Word/Byte Load Characteristics

| Symbol | Parameter | Min | Max | Units |
|---|---|---:|---:|---|
| tAS, tOES | Address, OE setup time | 0 | — | ns |
| tAH | Address hold time | 50 | — | ns |
| tCS | Chip select setup time | 0 | — | ns |
| tCH | Chip select hold time | 0 | — | ns |
| tWP | Write pulse width, WE or CE | 90 | — | ns |
| tDS | Data setup time | 50 | — | ns |
| tDH, tOEH | Data, OE hold time | 0 | — | ns |
| tWPH | Write pulse width high | 90 | — | ns |

---

### Program Cycle Characteristics

| Symbol | Parameter | Min | Typ | Max | Units |
|---|---|---:|---:|---:|---|
| tBP | Byte programming time | — | 10 | 50 | µs |
| tAS | Address setup time | 0 | — | — | ns |
| tAH | Address hold time | 50 | — | — | ns |
| tDS | Data setup time | 50 | — | — | ns |
| tDH | Data hold time | 0 | — | — | ns |
| tWP | Write pulse width | 90 | — | — | ns |
| tWPH | Write pulse width high | 90 | — | — | ns |
| tEC | Erase cycle time | — | — | 10 | seconds |

Important waveform note:

```text
OE must be high only when WE and CE are both low.
```

---

## Data Polling and Toggle Bit Timing

### Data Polling Characteristics

| Symbol | Parameter | Min | Units |
|---|---|---:|---|
| tDH | Data hold time | 10 | ns |
| tOEH | OE hold time | 10 | ns |
| tOE | OE to output delay | See AC read characteristics | ns |
| tWR | Write recovery time | 0 | ns |

---

### Toggle Bit Characteristics

| Symbol | Parameter | Min | Units |
|---|---|---:|---|
| tDH | Data hold time | 10 | ns |
| tOEH | OE hold time | 10 | ns |
| tOE | OE to output delay | See AC read characteristics | ns |
| tOEHP | OE high pulse | 150 | ns |
| tWR | Write recovery time | 0 | ns |

---

## Absolute Maximum Ratings

These are stress ratings only. Functional operation at these limits is not implied.

| Parameter | Rating |
|---|---|
| Temperature under bias | -55°C to +125°C |
| Storage temperature | -65°C to +150°C |
| All input voltages, including NC pins, with respect to ground | -0.6V to +6.25V |
| All output voltages with respect to ground | -0.6V to VCC + 0.6V |
| Voltage on OE with respect to ground | -0.6V to +13.5V |

Stresses beyond these ratings may cause permanent damage to the device.

---

## Package Information

### Package Types

| Package Code | Description |
|---|---|
| 32J | 32-lead, Plastic, J-leaded Chip Carrier Package (PLCC) |
| 32T | 32-lead, Thin Small Outline Package (TSOP) (8 x 20 mm) |
| 32V | 32-lead, Thin Small Outline Package (VSOP) (8 x 14 mm) |

---

### Package Identification

All package drawings show a pin 1 identifier.

For physical work:

```text
Always confirm pin 1 before wiring.
Always confirm package marking before wiring.
```

---

## Ordering Information

### Standard Package Options

| tACC (ns) | ICC Active (mA) | ICC Standby (mA) | Ordering Code | Package | Range |
|---:|---:|---:|---|---|---|
| 55 | 40 | 0.3 | AT49F512-55JI | 32J | Industrial (-40° to 85°C) |
| 55 | 40 | 0.3 | AT49F512-55TI | 32T | Industrial (-40° to 85°C) |
| 55 | 40 | 0.3 | AT49F512-55VI | 32V | Industrial (-40° to 85°C) |

---

### Green Package Options

| tACC (ns) | ICC Active (mA) | ICC Standby (mA) | Ordering Code | Package | Range |
|---:|---:|---:|---|---|---|
| 55 | 40 | 0.3 | AT49F512-55JU | 32J | Industrial (-40° to 85°C) |
| 70 | 40 | 0.3 | AT49F512-70TU | 32T | Industrial (-40° to 85°C) |
| 55 | 40 | 0.3 | AT49F512-55VU | 32V | Industrial (-40° to 85°C) |
| 70 | 40 | 0.3 | AT49F512-70VU | 32V | Industrial (-40° to 85°C) |

The standard boot block address range is:

```text
0x0000 to 0x1FFF
```

Devices with boot block in the higher address range were special-order parts.

---

## Hardware Programmer Design Notes

This project’s interactive simulator does not access physical hardware.

To build a real AT49F512 programmer, you need a system capable of driving the parallel bus.

Minimum signals required:

```text
A0-A15    = 16 address signals
I/O0-I/O7 = 8 bidirectional data signals
CE        = 1 control signal
OE        = 1 control signal
WE        = 1 control signal
```

Total:

```text
27 signals
```

If a data bus transceiver is used, add one direction-control signal:

```text
28 signals
```

[DESIGN RECOMMENDATION]

---

### Recommended Controller Options

#### Option A: 5V Microcontroller

A 5V microcontroller with enough GPIO pins is the simplest approach.

Examples:

```text
Arduino Mega 2560
ATmega2560-based boards
Other 5V-capable microcontrollers with enough pins
```

Advantages:

```text
Simple direct drive
No complex level shifting
Easy prototype wiring
USB serial communication to PC
```

[DESIGN RECOMMENDATION]

---

#### Option B: 3.3V Controller with Buffers

If using a 3.3V microcontroller or Raspberry Pi:

```text
Use buffering or level shifting.
```

Special care is required because:

```text
Flash inputs may accept 3.3V high logic depending on thresholds.
Flash outputs may drive near 5V and damage 3.3V GPIOs.
```

The datasheet specifies:

```text
VIL max = 0.8V
VIH min = 2.0V
```

However, robust design should use:

```text
74HCT-series buffers
Dedicated level translators
Bus transceivers
```

[DESIGN RECOMMENDATION]

---

### Recommended Programmer Building Blocks

```text
5V power supply
Decoupling capacitor near VCC
ZIF socket or package adapter
Address bus driver
Data bus transceiver
CE/OE/WE control
Microcontroller or computer-controlled GPIO
USB/serial interface to host PC
```

Recommended decoupling values, unless otherwise specified by your board design:

```text
100 nF ceramic capacitor close to VCC
10 µF bulk capacitor on the 5V rail
```

These are design recommendations, not datasheet-specified values.

[DESIGN RECOMMENDATION]

---

## Safe Engineering Workflow

Use this workflow before attempting erase or programming on real hardware.

```text
PHASE 1:  Identify chip/package
PHASE 2:  Build read-only hardware
PHASE 3:  Verify voltages
PHASE 4:  Read manufacturer/device ID
PHASE 5:  Read first bytes
PHASE 6:  Dump entire memory
PHASE 7:  Make backup
PHASE 8:  Check boot block lockout
PHASE 9:  Erase only after backup is confirmed
PHASE 10: Program test data
PHASE 11: Verify test data
PHASE 12: Modify real data
PHASE 13: Program modified image and verify fully
```

Do not jump directly to erase or programming before the read path is proven stable.

[DESIGN RECOMMENDATION]

---

## Example Experiments

### Experiment 1: Read one byte

Goal:

```text
Read address 0x0000.
```

Expected behavior:

```text
Repeated reads return the same value if hardware is stable.
```

---

### Experiment 2: Read product ID

Goal:

```text
Enter software product ID mode.
Read 0x0000 and 0x0001.
Exit product ID mode.
```

Expected:

```text
0x0000 = 0x1F
0x0001 = 0x03
```

---

### Experiment 3: Dump full memory

Goal:

```text
Read 0x0000 through 0xFFFF.
```

Expected output:

```text
65,536-byte binary file
```

Verify by reading twice and comparing.

---

### Experiment 4: Erase chip

Goal:

```text
Send chip erase command.
Wait or poll for completion.
Read chip.
```

Expected:

```text
Erasable bytes read as 0xFF.
```

If boot block lockout is enabled:

```text
0x0000 to 0x1FFF may retain old data.
```

---

### Experiment 5: Program one byte

Goal:

```text
After erase, program 0x55 to address 0x2000.
```

Command sequence:

```text
5555:AA
2AAA:55
5555:A0
2000:55
```

Expected:

```text
Readback = 0x55
```

---

### Experiment 6: Modify one byte

Example:

```text
Address = 0x1234
Original = 0xFF
New value = 0x42
```

Bit comparison:

```text
Original: 11111111
New:      01000010
```

All changed bits are:

```text
1 → 0
```

So this can be programmed if the byte is truly erased.

Example requiring erase:

```text
Current: 0x42
Desired: 0xFF
```

Bit comparison:

```text
Current: 01000010
Desired: 11111111
```

Some bits require:

```text
0 → 1
```

Therefore erase is required.

---

## Troubleshooting

### Read returns `0xFF` everywhere

Possible causes:

```text
Chip is genuinely erased
CE not actually low
OE not actually low
WE accidentally low
Address bus not connected
Data bus not connected
Data bus pulled high
Wrong pinout
No power
Chip damaged
```

---

### Read returns random values

Possible causes:

```text
Floating data bus
Floating address bus
Bad ground
No decoupling
Signal integrity problem
Bus contention
Loose socket
Wrong pin mapping
Read timing too aggressive
```

---

### Programming fails

Possible causes:

```text
Chip not erased
Boot block lockout enabled
Wrong command sequence
Wrong command addresses
Wrong command data
WE pulse too short
CE not low during command
OE not high during command
Data bus not driven correctly
VCC droop
Address/data setup/hold violation
Target byte requires 0 → 1 change without erase
```

---

### Product ID fails

Possible causes:

```text
Command sequence not correctly written
CE/OE/WE states incorrect
OE not high during command writes
WE pulse too short
Data bus not driven
Address bus wiring error
Chip is damaged
Wrong device
```

Expected ID:

```text
Manufacturer = 0x1F
Device       = 0x03
```

---

## Source Discipline

This project tries to separate information sources clearly.

| Label | Meaning |
|---|---|
| `[DATASHEET]` | Directly supported by the AT49F512 datasheet |
| `[ENGINEERING KNOWLEDGE]` | General electronics or Flash memory knowledge |
| `[DESIGN RECOMMENDATION]` | Practical recommendation for building/testing |
| `[INFERENCE]` | Derived from datasheet behavior and normal hardware operation |

When building real hardware, always verify against the actual chip marking, package, and datasheet drawing.

---

## Disclaimer

This project is for educational purposes.

Working with real Flash memory, sockets, power supplies, and programmers can damage components if wired incorrectly.

There is no warranty, express or implied.

Use the information at your own risk.

This project is not affiliated with Atmel or Microchip unless otherwise stated.

All product names, trademarks, and registered trademarks are property of their respective owners.

---

## License

Add your chosen license here.

For example:

```text
MIT License
```

or:

```text
CC-BY-4.0 for documentation
MIT for code
```

If no license is provided, default copyright terms may apply depending on jurisdiction.