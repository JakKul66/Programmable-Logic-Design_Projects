# Johnson Counter – 4-bit VHDL Implementation

A 4-bit Johnson (twisted ring) counter implemented in VHDL, designed and verified on the **Basys 3 FPGA board** (Xilinx xc7a35tcpg236-1).

## Overview

The counter cycles through 8 unique states using the Johnson encoding scheme, where on each clock edge the MSB's complement is shifted into the LSB. The design uses a mechanical button as the clock source and includes asynchronous reset.

## I/O

| Signal | Board Pin | Direction | Description |
|--------|-----------|-----------|-------------|
| `clk_i` | BTNC (center) | Input | Clock (manual button press) |
| `rst_i` | BTNR (right) | Input | Asynchronous reset |
| `led_o(0)` | LD0 | Output | Johnson counter bit 0 |
| `led_o(1)` | LD1 | Output | Johnson counter bit 1 |
| `led_o(2)` | LD2 | Output | Johnson counter bit 2 |
| `led_o(3)` | LD3 | Output | Johnson counter bit 3 |

## Top-level Entity
```vhdl
entity top is
  Port ( clk_i : in  STD_LOGIC;
         rst_i : in  STD_LOGIC;
         led_o : out STD_LOGIC_VECTOR (3 downto 0));
end top;
```

## Notes

- Since the clock is driven by a mechanical button, contact bounce may produce multiple clock pulses per press.
- Constraints file for Basys 3: `iup4.xdc`
- Functional simulation covers all 8 counter states and includes two reset events (one at startup, one mid-sequence).
- The following Vivado warnings are expected and can be safely ignored in this design:
  - `[Power 33-232]` – no user-defined clocks
  - `[Timing 38-313]` – no timing constraints
  - `[Place 46-29]` – not in timing mode
  - `[Place 30-574]` – poor placement for IO pin to BUFG routing