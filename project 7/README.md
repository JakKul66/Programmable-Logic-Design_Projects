# Coin Flip – VHDL Debouncing Fix

A "coin flip" game implemented in VHDL, deployed and debugged on the **Basys 3 FPGA board** (Xilinx xc7a35tcpg236-1).

## Overview

The circuit simulates a coin toss: while button BTNC is held, LEDs LD0–LD15 rapidly alternate between two states at 100 MHz — **HEADS** (all LEDs on) and **TAILS** (all LEDs off). Releasing the button freezes the display on the randomly landed state.

The original provided code (`moneta.vhd`) contains a bug observable after a number of button presses. The task was to identify the root cause and fix it within the designated modification area.

## The Bug

The original design lacked a **button debouncer**. Mechanical button releases produce contact bounce — multiple rapid signal transitions — which cause the circuit to register extra clock edges and land on an incorrect or unstable state instead of the intended one.

## The Fix

A debouncing circuit was added inside the marked modification area:
```vhdl
-- Początek obszaru modyfikacji kodu
-- [debouncer logic here]
-- Koniec obszaru modyfikacji kodu
```

The debouncer filters out spurious transitions on `btn_i` by requiring the signal to remain stable for a set number of clock cycles before passing it through to the rest of the logic.

## I/O

| Signal | Board Pin | Direction | Description |
|--------|-----------|-----------|-------------|
| `clk_i` | FPGA clock | Input | 100 MHz system clock |
| `btn_i` | BTNC (center) | Input | Game trigger button |
| `led_o` | LD15–LD0 | Output | Coin flip result (all on = heads, all off = tails) |

## Top-level Entity
```vhdl
entity moneta is
  Port ( clk_i : in  STD_LOGIC;
         btn_i : in  STD_LOGIC;
         led_o : out STD_LOGIC_VECTOR (15 downto 0));
end moneta;
```

## Notes

- Constraints file for Basys 3: `iup7.xdc`
- The bug typically manifests after a dozen or so button presses.
- No changes were made outside the designated modification area.