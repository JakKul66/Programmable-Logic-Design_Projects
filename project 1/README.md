# 8-bit Parity Generator (FPGA / VHDL)

This project implements an **8-bit parity generator** designed in **VHDL** and deployed on the **Basys 3 FPGA board**. The system reads an 8-bit input word from hardware switches and determines whether the number of `1` bits is **even** or **odd**.

The result is displayed on a **7-segment display**:

- `E` – even number of `1` bits  
- `O` – odd number of `1` bits  

## How It Works

- The 8-bit input word is provided through switches **SW0–SW7**.
- The circuit counts the number of `1` bits in the input.
- Based on the parity:
  - **E** is displayed for even parity
  - **O** is displayed for odd parity
- The result is shown on the **AN0 seven-segment display** of the Basys 3 board.

The board uses **four multiplexed seven-segment displays with a common anode**, where:
- Segments **A–G** are shared between displays.
- Displays are activated through signals **AN0–AN3** (active low).

## Requirements Implemented

- Functional simulation of the design.
- Hardware verification on the **Basys 3 FPGA board**.
- Simulation scenario where switches are turned on sequentially every **100 ms**, starting from all OFF until all switches are ON.

## Interface

### Inputs
- `sw_i[7:0]` – 8-bit input word from switches

### Outputs
- `led7_an_o[3:0]` – seven-segment display anode control
- `led7_seg_o[7:0]` – segment control signals (A–G + DP)

## Technologies Used

- **VHDL**
- **Vivado**
- **Basys 3 FPGA (xc7a35tcpg236-1)**