# Traffic Light Controller using VLSI on Artix-7 FPGA

## Project overview
This project implements a two-road traffic-light controller using Verilog HDL and a finite-state machine (FSM). It is intended for synthesis and implementation on an AMD/Xilinx Artix-7 FPGA using Vivado Design Suite.

## Features
- North-South and East-West traffic directions
- Green, yellow and red signal outputs for both roads
- One-second all-red clearance between road changes
- Parameterized clock frequency and signal durations
- Active-high synchronous reset
- Verilog testbench for functional simulation

## State sequence
1. North-South green, East-West red
2. North-South yellow, East-West red
3. Both directions red (clearance)
4. East-West green, North-South red
5. East-West yellow, North-South red
6. Both directions red (clearance), then repeat

## Repository structure
```text
traffic_light_artix7/
├── rtl/
│   └── traffic_light_top.v
├── sim/
│   └── tb_traffic_light_top.v
├── constraints/
│   └── board_template.xdc
└── README.md
```

## Default timing
The RTL assumes a 100 MHz input clock:
- Green: 10 seconds
- Yellow: 3 seconds
- All-red clearance: 1 second

Change `CLK_FREQ_HZ`, `GREEN_SEC`, and `YELLOW_SEC` when instantiating the module if your board clock or desired timing differs.

## Vivado implementation
1. Create a new RTL project in Vivado.
2. Select the exact Artix-7 FPGA part fitted to your development board.
3. Add `rtl/traffic_light_top.v` as a design source.
4. Add `sim/tb_traffic_light_top.v` as a simulation source.
5. Add `constraints/board_template.xdc` as a constraints file.
6. Replace the placeholder pins in the XDC using the official master XDC for your exact board. Never guess pin locations.
7. Set `traffic_light_top` as the top module.
8. Run **Run Simulation**, then **Run Synthesis** and **Run Implementation**.
9. Generate the bitstream and program the FPGA.

## Simulation
The testbench uses shortened timing values so that the full sequence can be checked quickly. It also checks that both roads are never green at the same time.

## Top-level I/O
| Signal | Direction | Description |
|---|---|---|
| `clk` | Input | FPGA system clock |
| `reset` | Input | Active-high reset |
| `ns_red` | Output | North-South red LED |
| `ns_yellow` | Output | North-South yellow LED |
| `ns_green` | Output | North-South green LED |
| `ew_red` | Output | East-West red LED |
| `ew_yellow` | Output | East-West yellow LED |
| `ew_green` | Output | East-West green LED |

## Important hardware note
The XDC file is deliberately a template because Artix-7 boards use different FPGA packages and pin assignments. Obtain the master XDC for your exact board (for example, Basys 3 or Nexys A7) and map the six LED outputs, clock, and reset to the correct board pins before generating a bitstream.

## Tools
- Verilog HDL
- AMD/Xilinx Vivado Design Suite
- Artix-7 FPGA development board


## Nexys A7 board setup

A board-specific constraint file is provided at
`constraints/Nexys-A7-Traffic-Light.xdc`.

This mapping uses:
- 100 MHz clock: E3
- SW0 reset input: J15
- NS red/yellow/green: LD0/LD1/LD2
- EW red/yellow/green: LD3/LD4/LD5
