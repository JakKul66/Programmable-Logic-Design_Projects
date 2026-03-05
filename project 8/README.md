# Simple Stopwatch – VHDL Implementation

A centisecond-precision stopwatch implemented in VHDL, designed and verified on the **Basys 3 FPGA board** (Xilinx xc7a35tcpg236-1). Reuses the `display` component from the previous lab exercise.

## Overview

Each press of BTNC cycles the stopwatch through three states:
```
START → STOP → RESET → (back to START)
```

Elapsed time is shown on the 4-digit 7-segment display as **SS.DD** (seconds . centiseconds). On overflow past 59.99 s, a special symbol (e.g. `--.--`) is shown. The BTNC input is debounced to handle up to ~50 ms of contact bounce.

## I/O

| Signal | Board Pin | Direction | Description |
|--------|-----------|-----------|-------------|
| `clk_i` | FPGA clock | Input | 100 MHz system clock |
| `rst_i` | BTNR (right) | Input | Asynchronous reset → display `00.00`, wait for start |
| `start_stop_button_i` | BTNC (center) | Input | Start / Stop / Reset button (debounced) |
| `led7_an_o` | AN3–AN0 | Output | 7-segment anode select |
| `led7_seg_o` | A–G, DP | Output | 7-segment segment select |

## Top-level Entity
```vhdl
entity top is
  Port ( clk_i               : in  STD_LOGIC;
         rst_i               : in  STD_LOGIC;
         start_stop_button_i : in  STD_LOGIC;
         led7_an_o           : out STD_LOGIC_VECTOR (3 downto 0);
         led7_seg_o          : out STD_LOGIC_VECTOR (7 downto 0));
end top;
```

## Architecture

The design is composed of three main blocks:

- **Debouncer** – filters contact bounce on `start_stop_button_i`, assuming up to 50 ms bounce duration.
- **Stopwatch counter** – counts centiseconds and seconds in BCD, handles overflow detection at 59.99 s.
- **`display` component** – reused from the previous exercise; receives a 32-bit digit input and drives the multiplexed 7-segment display at 1 kHz.

## Notes

- Overflow (> 59.99 s) is indicated by a dedicated symbol on the display (e.g. `--.--`); pressing BTNC clears it.
- Asynchronous reset via BTNR works at any point, including during active counting.
- Constraints file for Basys 3: `iup8.xdc`
- Functional simulation (100 MHz clock) demonstrates start, stop at 1.XY s (where XY = last two digits of student index), and reset via `start_stop_button_i`.
- Board verification additionally demonstrates overflow, overflow clear, and asynchronous reset during counting.