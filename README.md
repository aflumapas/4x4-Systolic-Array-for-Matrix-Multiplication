# 4×4 Systolic Array for Matrix Multiplication (Verilog)

A high-throughput hardware accelerator implemented in Verilog, designed for matrix multiplication and deep learning workloads (inspired by Tensor Processing Unit architectures).

## Overview

This project implements a fully pipelined **$4 \times 4$ Systolic Array** consisting of 16 interconnected Processing Elements (PEs). By streaming operands horizontally and vertically across the grid, the architecture maximizes data reuse, minimizes memory bandwidth bottlenecks, and performs parallel Multiply-Accumulate (MAC) operations efficiently.

---

## System Architecture

The design is split into two primary hierarchical modules:

### 1. Processing Element (`PE`)
The fundamental compute building block of the array:
* **Multiply-Accumulate (MAC):** Multiplies two signed 8-bit inputs (`inputA` and `inputB`) and accumulates the product into a 32-bit register (`accum_total`) to prevent overflow over extended compute cycles.
* **Data Pipelining:** Passes inputs straight through to adjacent nodes (`outputA` and `outputB`) on every clock cycle.
* **Asynchronous Reset:** Clears all internal registers and accumulators instantly to prevent data corruption.

### 2. Array (`array`)
* Connects a $4 \times 4$ grid of 16 PEs (`PE_1` through `PE_16`).
* Manages row and column input distribution and routes 16 independent 32-bit parallel accumulation outputs (`output_accum0` to `output_accum15`).

---

## Key Features

* **Pipelined Data Flow:** Operands ripple through neighboring nodes to maximize hardware concurrency and throughput.
* **Signed Arithmetic:** Native support for signed 8-bit operands with full 32-bit precision accumulation.
* **Modular Design:** Clean separation between primitive PE logic and top-level routing, making it easy to scale to larger grids ($8 \times 8$, $16 \times 16$, etc.).
* **Robust Reset Logic:** Asynchronous active-high reset for reliable state initialization.

---

## File Structure

```text
├── array.v          # Top-level 4x4 systolic array module and PE instantiations
├── PE.v             # Processing Element module (MAC + data pass-through)
└── README.md        # Project documentation
