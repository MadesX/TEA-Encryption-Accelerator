# TEA Encryption Accelerator

A hardware-accelerated implementation of the Tiny Encryption Algorithm (TEA) using SystemVerilog, deployed on the Digilent Nexys A7 FPGA board.  
The system includes a Wishbone-compatible register interface, bi-directional I/O, and an integrated C software controller running on an embedded softcore processor.

# Project Overview

This project implements two 32-bit words, representing a 64-bit block encryption accelerator for the TEA cipher using SystemVerilog on the Nexys A7 board.
It is controlled by a custom C firmware developed in SEGGER Embedded Studio, compiled for a RISC-V softcore or equivalent.

The design provides fast and lightweight encryption, combining both hardware and software elements. You can encrypt and decrypt in both hardware and software or a combination of the two.

The full system includes:
  * SystemVerilog Hardware Modules for TEA core, control logic, and Wishbone register file.
  * Testbench for simulation and debugging in Vivado.
  * C Software to control and communicate with the accelerator (run via SEGGER).
  * Vivado Project Files for deployment on the Nexys A7 board.

# Key Features

* Fully Hardware-Accelerated TEA Cipher

  * Two 32-bit words, representing a 64-bit block input with four 32-bit words forming 128-bit key.
  * Executes 32 encryption rounds in hardware.

* Bidirectional I/O Handling
  
  * vin0 and vin1 support shared input/output operation.
  * Finite State Machine manages data direction.

* Wishbone-Compatible Register File

  * For control, status, and data movement.
  * Allows easy integration with CPU/software.

* Simulation & Debugging

  * Includes SystemVerilog testbench to validate encryption correctness.
  * Shows encrypted outputs and register interaction.

* C Software Integration

  * Written in SEGGER Embedded Studio.
  * Manages hardware interaction, triggers encryption, reads back text.

# How to Use
**Important :** Keep all files in their original folders.  

**0. Download And Extract**  

   * Download all zip files and extarct `RVFPGA_EL2_03.zip`.

**1. Vivado Hardware Setup**

   - Launch Vivado.
   - Open Hardware Manager and Program the Nexys A7 board using the existing bitstream file. 

**2. Software Setup in SEGGER Embedded Studio**  

   - Launch SEGGER.
   - Load project TEA from folder sw -> TEA.
   - By default, encryption and decryption are performed using hardware. However, you can use #define HWEncrypt and #define HWDecrypt to choose whether to run them using hardware, software, or a combination of both.
   - Compile and upload the program via SEGGER J-Link.

**Modify System**

   - Load Vivado project accelerator from folder wishbone_simulation -> accelerator to change core logic or overall behavior and to run simulation using testbench.
   - Load Vivado project nexysA7card from folder vivado to recreate bitfile after changes in accelerator project.
   - Load SEGGER project TEA from folder sw -> TEA to change hardware interaction.

# Register Map

| Register Name | Address      | Bits | Access | Core Direction | Accelerator Reg |
| ------------- | ------------ | ---- | ------ | -------------- | --------------- |
| `REG_VIN0`    | `0x80001300` | 32   | R/W    | I/O            | I/O             |
| `REG_VIN1`    | `0x80001304` | 32   | R/W    | I/O            | I/O             |
| `REG_K0`      | `0x80001308` | 32   | Read   | Input          | Output          |
| `REG_K1`      | `0x8000130C` | 32   | Read   | Input          | Output          |
| `REG_K2`      | `0x80001310` | 32   | Read   | Input          | Output          |
| `REG_K3`      | `0x80001314` | 32   | Read   | Input          | Output          |
| `REG_START`   | `0x80001318` | 1    | R/W    | Input          | Output          |
| `REG_SELECT`  | `0x8000131C` | 1    | Read   | Input          | Output          |
| `REG_DONE`    | `0x80001320` | 1    | Write  | Output         | Input           |

# Notes

- Designed for the Digilent Nexys A7 board (Artix-7 FPGA).
- Can be adapted for other platforms supporting Wishbone or AXI.
- The TEA cipher is used for educational purposes and is not secure by modern standards.
- FSM-based direction control handles shared I/O (vin0/vin1).
- Register map is aligned with standard memory-mapped I/O patterns.

# Minimum Requirements

* Hardware:
  * Digilent Nexys A7 board
  * JTAG programming cable (for FPGA)
  * USB-UART or SEGGER J-Link (for software)

* Software:
  * Vivado 2020.2 or later
  * SEGGER Embedded Studio for RISC-V or ARM

# License

This project is licensed under a custom license:

You may use, copy, and modify the code for **personal or non-profit purposes** for free.  
If you wish to use the code in **any commercial or for-profit product**, you must contact the author and may be required to pay a fee or share profits.

© 2025 Matan Sides  
All rights reserved.
