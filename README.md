# Risc---V-KNN

Lightweight Single-Cycle RISC-V Processor with Custom Instruction Set Extension for Accelerated KNN Computation
Overview

This project presents the design and implementation of a lightweight 32-bit single-cycle RISC-V processor integrated with a custom instruction set extension to accelerate K-Nearest Neighbors (KNN) distance computation. The processor introduces a dedicated instruction named KDIST, enabling efficient hardware-level execution of Euclidean distance calculations commonly used in machine learning workloads.

Traditional RISC-V execution requires multiple arithmetic instructions to perform KNN distance computation, leading to increased instruction count and execution latency. To address this limitation, the proposed architecture combines multiple arithmetic operations into a single custom instruction, reducing computational overhead and improving execution efficiency.

The processor is designed and verified using Verilog HDL, with functional validation performed through RTL simulation in Vivado.

Problem Statement

In a conventional RISC-V processor, KNN distance computation involves executing several arithmetic operations sequentially, including subtraction, multiplication, and addition.

Example sequence:

SUB
SUB
MUL
MUL
ADD

This approach increases:

Instruction count
Execution cycles
Processing latency

To improve efficiency, this project introduces a custom KDIST instruction, allowing the processor to execute the KNN distance calculation in a single instruction.

Objectives
Design and implement a lightweight 32-bit RISC-V processor for instruction execution.
Integrate a custom KDIST instruction for accelerating KNN distance computation.
Improve computational efficiency by reducing instruction count and execution cycles for KNN operations.
Proposed Solution

The proposed processor extends the standard RV32I-inspired architecture with a custom instruction called KDIST.

The KDIST instruction computes the squared Euclidean distance:

(a
1
	​

−b
1
	​

)
2
+(a
2
	​

−b
2
	​

)
2

Instead of performing multiple arithmetic operations separately, the processor executes the calculation using a dedicated KNN hardware unit, thereby reducing software overhead.

Processor Architecture

The processor consists of the following major modules:

1. Program Counter (PC)
Maintains instruction sequence.
Increments by 4 after each clock cycle.
Supports reset functionality.
2. Instruction Memory
Stores preloaded instructions.
Fetches instructions using the Program Counter address.
3. Control Unit
Decodes instruction opcode.
Generates processor control signals.
Activates the KDIST operation using knn_enable.
4. Register File
Contains 32 general-purpose registers.
Performs register read and write operations.
5. Arithmetic Logic Unit (ALU)
Executes standard arithmetic operations such as ADD and SUB.
6. KNN Unit (KDIST Module)
Computes squared Euclidean distance using custom hardware logic.
7. Write-Back Unit
Selects between ALU result and KNN output for register update.
Custom Instruction Format

The processor introduces a custom opcode for KDIST:

Field	Description
Opcode	1111111
Function	KNN Distance Computation
Type	Custom R-Type Instruction

Instruction format:

funct7 | rs2 | rs1 | funct3 | rd | opcode

Example:

1111111_00010_00001_000_00101_1111111

Meaning:

KDIST x5, x1, x2
Project Workflow
Instruction Fetch
        ↓
Instruction Decode
        ↓
Register Read
        ↓
KDIST Detection
        ↓
KNN Distance Computation
        ↓
Write Back Result
Simulation and Verification

The processor was functionally verified through waveform simulation in Xilinx Vivado.

Sample Execution
Signal	Value
PC	8
read_data1	5
read_data2	7
b1	2
b2	3
KDIST Result	25

Distance Computation:

(5−2)
2
+(7−3)
2
=3
2
+4
2
=9+16
=25

The simulation confirms correct execution of the custom instruction and successful integration of the KNN hardware block.

Comparison with Conventional RISC-V
Parameter	Conventional RISC-V	Proposed RISC-V with KDIST
KNN Computation	Multiple Instructions	Single Custom Instruction
Instruction Count	Higher	Reduced
Execution Cycles	More	Less
Latency	Higher	Lower
Hardware Support for KNN	Not Available	Integrated
Computational Efficiency	Moderate	Improved
Tools and Technologies Used
Verilog HDL — Hardware Design
Xilinx Vivado — Simulation and Verification
GTKWave — Waveform Analysis
RISC-V Architecture — Processor Design Foundation
Results

The proposed processor successfully:

Implements a lightweight single-cycle 32-bit RISC-V processor
Integrates a custom KDIST instruction
Reduces instruction count for KNN computation
Demonstrates improved execution efficiency through custom ISA extension
Produces correct KNN distance results verified via simulation
Future Scope

Potential future enhancements include:

Implementation of pipelined architecture
FPGA-based hardware deployment
Support for additional machine learning instructions
Optimization for larger KNN datasets
Power and timing analysis on physical hardware
Repository Structure
├── cpu_top.v
├── pc.v
├── instruction_memory.v
├── control_unit.v
├── register_file.v
├── alu.v
├── knn_unit.v
├── testbench.v
├── waveform/
└── README.md
Conclusion

This project demonstrates the successful implementation of a lightweight RISC-V processor enhanced with a custom instruction for KNN acceleration. By introducing the KDIST instruction, the processor reduces instruction overhead and improves computational efficiency for distance-based machine learning operations. The design validates the practicality of extending RISC-V architecture with domain-specific instructions for application-oriented acceleration.
