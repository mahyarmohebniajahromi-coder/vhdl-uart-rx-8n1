# VHDL UART Receiver (RX) – 8N1 + Testbench

FPGA-ready UART **RX** module written in VHDL. Implements standard **8N1** framing:
**1 start bit (0)**, **8 data bits (LSB-first)**, **1 stop bit (1)**.
Includes a simulation-ready **testbench** for functional verification.

> Note: This RX can be used standalone or paired with a UART TX module for a complete UART core.

## Features
- UART RX frame decoding (8N1): start detection → data sampling → stop-bit check
- Mid-bit sampling strategy (robust against small jitter)
- Configurable baud rate via generics (clock frequency + baud rate)
- Clean, synthesizable RTL (no vendor-specific primitives)
- Testbench included (ModelSim/Vivado simulator friendly)
- Output handshake signals (e.g., `data_valid` / `rx_done`) for easy integration

## UART Frame (8N1)
Idle line = '1'  
Start bit = '0'  
Data bits = D0..D7 (LSB-first)  
Stop bit = '1'

## File Structure
- `src/` : UART RX RTL (VHDL)
- `tb/`  : Testbench (VHDL)
- `sim/` : (optional) scripts/wave configs
- `docs/`: (optional) frame diagram / notes

## Quick Start (Simulation)
1. Create a new project in **ModelSim** or **Vivado**.
2. Add VHDL sources from `src/` and the testbench from `tb/`.
3. Set generics:
   - `G_CLK_HZ = [e.g., 50_000_000]`
   - `G_BAUD   = [e.g., 115200]`
4. Compile and run simulation.
5. In the waveform, verify:
   - RX detects start bit
   - Samples 8 data bits correctly (LSB-first)
   - Validates stop bit and asserts `data_valid`/`done`
   - Output byte matches expected data

## Interface (Typical)
This RX module typically provides:
- `clk`, `rst`
- `rxd` (serial input)
- `data_out(7 downto 0)`
- `data_valid` / `done` (one-cycle pulse or level, depending on design)
- `frame_error` (optional) when stop bit is invalid

> Check the entity in `src/uart_rx.vhd` for exact signal names.

## Design Notes
- RX waits for a falling edge (start bit), then samples in the center of each bit period.
- For noisy links, you can extend this design to oversampling (e.g., 8x/16x), but this implementation keeps the logic lightweight.

## Need Custom FPGA/VHDL Work?
If you need help integrating UART RX/TX into your FPGA design (or any RTL/VHDL work):
- RTL design & verification
- Vivado/ModelSim debug, timing closure
- Practical VHDL training (Farsi/English)

Email: mahyar.mohebnia.jahromi@gmail.com  
Telegram: https://t.me/mahyar_mohebnia
