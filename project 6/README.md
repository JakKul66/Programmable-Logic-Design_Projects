# Frequency Divider by N – VHDL Implementation

A parameterizable frequency divider implemented in VHDL, designed and verified on the **Basys 3 FPGA board** (Xilinx xc7a35tcpg236-1).

## Overview

The divider splits the input clock frequency by a configurable factor **N**, with an output duty cycle as close to 50% as possible:
- **Even N** – exact 50% duty cycle
- **Odd N** – 50% duty cycle with ±1 input clock period accuracy

The value of N can be easily changed via a `constant` or `generic`. The design includes asynchronous reset that clears the internal counter and drives the output low.

## I/O

| Signal | Board Pin | Direction | Description |
|--------|-----------|-----------|-------------|
| `clk_i` | FPGA clock | Input | 100 MHz input clock |
| `rst_i` | BTNR (right) | Input | Asynchronous reset |
| `led_o` | LD0 | Output | Divided clock output |

## Top-level Entity
```vhdl
entity top is
  Port ( clk_i : in  STD_LOGIC;
         rst_i : in  STD_LOGIC;
         led_o : out STD_LOGIC);
end top;
```

## Configuration

| Use case | N value |
|----------|---------|
| Functional simulation | Sum of digits of student index number |
| Board verification (1 Hz LED blink) | `100_000_000` |

## Notes

- Input clock frequency: 100 MHz.
- Constraints file for Basys 3: `iup5.xdc`
- Functional simulation shows both the input clock and divider output on the same waveform, covering several full output cycles and two reset events (one at startup, one mid-sequence).