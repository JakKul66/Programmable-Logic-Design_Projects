# FPGA Lab Projects – Basys 3 (Xilinx xc7a35tcpg236-1)

A collection of 8 VHDL projects developed for the **Basys 3 FPGA board** as part of digital design lab coursework. Each project is implemented in VHDL and verified using Vivado with functional simulation and on-board testing.

## Projects

| Directory | Project | Constraints |
|-----------|---------|-------------|
| `proj1/` | Combinational logic – ones counter (4-bit input → 7-segment display) | `iup2.xdc` |
| `proj2/` | Parity bit generator (8-bit input → E/O on 7-segment display) | `iup1.xdc` |
| `proj3/` | 3-bit Gray code counter | `iup3.xdc` |
| `proj4/` | 4-bit Johnson counter | `iup4.xdc` |
| `proj5/` | Frequency divider by N (50% duty cycle, configurable N) | `iup5.xdc` |
| `proj6/` | Multiplexed 4-digit 7-segment LED display controller | `iup6.xdc` |
| `proj7/` | Coin flip game with button debouncing fix | `iup7.xdc` |
| `proj8/` | Centisecond stopwatch with 7-segment display | `iup8.xdc` |

## Tools & Requirements

- **HDL:** VHDL
- **IDE:** Xilinx Vivado (any version supporting Artix-7)
- **Board:** Digilent Basys 3 (FPGA: xc7a35tcpg236-1)
- **Clock:** 100 MHz on-board oscillator (where applicable)

## Getting Started

1. Clone the repository:
```bash
   git clone https://github.com/jakkul66/Programmable-Logic-Design_Projects.git
```

2. Open Vivado and create a new project.

3. Add the source files from the relevant `projX/` directory:
   - `top.vhd` (or `moneta.vhd` for proj7) – main design file
   - `*.xdc` – board constraints file

4. Run **Simulation → Run Functional Simulation** to verify behavior.

5. Run **Synthesis → Implementation → Generate Bitstream**.

6. Connect the Basys 3 board via USB and program it using **Open Hardware Manager → Program Device**.

## Repository Structure
```
.
├── proj1/   # Combinational ones counter
├── proj2/   # Parity generator
├── proj3/   # Gray code counter
├── proj4/   # Johnson counter
├── proj5/   # Frequency divider
├── proj6/   # LED display controller
├── proj7/   # Coin flip (debouncing)
└── proj8/   # Stopwatch
```

## Notes

- Projects that use a mechanical button as clock source (proj3, proj4) may be affected by contact bounce — this is expected and noted in the respective READMEs.
- The `display` component developed in proj6 is reused as a subcomponent in proj8.
- The debounce fix in proj7 is confined to the marked modification area in `moneta.vhd`.
- The following Vivado warnings are harmless and can be ignored across all projects:
  - `[Power 33-232]` – no user-defined clocks
  - `[Timing 38-313]` – no timing constraints
  - `[Place 46-29]` – not in timing mode
  - `[Place 30-574]` – poor placement for IO pin to BUFG routing