# ⚡ 16-bit General Purpose Processor (GPP) + Tensor ASIP Extension

A high-performance hardware design project featuring a **16-bit General Purpose Processor** implemented in structural **Verilog**. Inspired by the classical **IAS-style accumulator-based architecture**, this System-on-Chip (SoC) integration includes a modular CPU, synchronized I/O via handshaking, and a specialized **ASIP extension** for hardware-accelerated tensor operations.

## ✨ Core Technical Features

* **Instruction Set:** Fixed-length 16-bit instructions with a 6-bit opcode.
* **Datapath Architecture:** Accumulator-based design featuring a compact register set (`AC`, `X`, `Y`).
* **Control Logic:** Centralized **One-Hot encoded FSM** for optimized state transition and high-frequency execution.
* **Conditionals:** Dedicated `FLAGS` register (`N`, `Z`, `C`, `V`) supporting complex branching logic.
* **Memory Model:** Unified memory space (Von Neumann architecture) for instructions and data.
* **I/O Synchronization:** Robust **Handshake-based protocol** ensuring safe and reliable data transfers.
* **Acceleration:** Domain-specific **ASIP extension** for high-speed tensor and matrix operations.

## 🧱 System Architecture & Modules

### 🧩 SoC Components
* **CPU Core:** Manages the full Fetch → Decode → Execute cycle.
* **Memory Unit:** 512 × 16-bit word-addressable memory (initialized to zero).
* **I/O Units:** Input and Output modules utilizing a request/acknowledge synchronization mechanism.

### 🧾 Internal Register Set
| Register | Description |
| :--- | :--- |
| **PC** | Program Counter |
| **SP** | Stack Pointer (downward growing) |
| **AC** | Accumulator (Primary register) |
| **X, Y** | General-purpose registers |
| **FLAGS** | Status register (Negative, Zero, Carry, Overflow) |
| **IR / AR** | Instruction Register / Address Register |

### 🧮 ALU Subsystem
The ALU is a self-contained unit with its own **dedicated Control Unit (FSM)**. It supports multi-cycle iterative operations (using internal `A`, `Q`, `M` registers) and provides real-time updates to the `FLAGS` register to enable conditional execution.

## 🔌 I/O Handshake Protocol
To prevent data loss and ensure synchronization between the fast CPU and slower peripherals:
* **Input:** CPU asserts `inp_req` → Unit replies with `inp_ack` + valid data.
* **Output:** CPU asserts `out_req` + drives data → Unit replies with `out_ack` after successful reception.

## ⚡ ASIP Extension (Tensor Accelerator)
The ASIP adds specialized instructions that operate directly on memory-resident tensors, significantly reducing software loop overhead:
* `ADDM` / `SUBM`: Tensor addition and subtraction.
* `MULM`: High-complexity Tensor/Matrix multiplication.
* `ELMULM`: Element-wise multiplication.
* *Internal Logic:* Each ASIP instruction expands into a custom micro-operation sequence, transparent to the programmer.

## 🛠 Technologies & Tools
* **HDL:** Verilog (Modular & Structural coding style).
* **Simulation:** GTKWave (Waveform inspection and timing analysis).
* **Verification:** Comprehensive testbenches for module-level and system-level validation.

## 📂 Repository Structure
```text
├── src/           # Verilog RTL source files (CPU, ALU, Memory, ASIP)
├── tb/            # Testbenches for module & system verification
├── programs/      # Assembly-level test programs & instruction encodings
└── docs/          # Architecture diagrams, FSM flowcharts, and ISA details
