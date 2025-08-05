# CPU Simulator

A comprehensive CPU simulator written in C that implements a custom instruction set architecture with pipelined execution, register file, memory management, and status register functionality.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Instruction Set](#instruction-set)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Technical Details](#technical-details)
- [Status Register (SREG)](#status-register-sreg)
- [Memory Layout](#memory-layout)
- [Pipeline Stages](#pipeline-stages)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This project implements a complete CPU simulator that demonstrates computer architecture concepts including:

- **Instruction Fetch-Decode-Execute Pipeline**: 3-stage pipeline implementation
- **Register File**: 64 8-bit registers (R0-R63)
- **Memory System**: Separate instruction and data memory
- **Status Register**: 5-bit status register with flags
- **Custom Instruction Set**: 12 different instruction types
- **Branch and Jump Support**: Conditional and unconditional branching

## ✨ Features

- **Pipelined Execution**: 3-stage pipeline (Fetch, Decode, Execute)
- **Register Operations**: Arithmetic, logical, and data movement operations
- **Memory Operations**: Load and store instructions
- **Status Flags**: Zero, Negative, Carry, Overflow, and Sign flags
- **Branch Prediction**: Simple branch handling
- **Instruction Memory**: 1024 instruction capacity
- **Data Memory**: 2048 byte data memory
- **File-based Input**: Instructions loaded from text file

## 🏗️ Architecture

### Core Components

- **Register File**: 64 × 8-bit registers
- **Instruction Memory**: 1024 × 16-bit instructions
- **Data Memory**: 2048 × 8-bit data locations
- **Program Counter (PC)**: 16-bit instruction pointer
- **Status Register (SREG)**: 5-bit status flags
- **Pipeline Registers**: Internal state management

### Pipeline Stages

1. **Fetch Stage**: Retrieves instructions from instruction memory
2. **Decode Stage**: Parses instruction fields (opcode, registers, immediate)
3. **Execute Stage**: Performs arithmetic, logical, and memory operations

## 🖥️ Instruction Set

| Opcode | Instruction | Description | Format |
|--------|-------------|-------------|---------|
| 0 | ADD | Add registers | `ADD R1 R2` |
| 1 | SUB | Subtract registers | `SUB R1 R2` |
| 2 | MUL | Multiply registers | `MUL R1 R2` |
| 3 | MOVI | Move immediate | `MOVI R1 123` |
| 4 | BEQZ | Branch if equal to zero | `BEQZ R1 10` |
| 5 | ANDI | AND with immediate | `ANDI R1 255` |
| 6 | EOR | Exclusive OR registers | `EOR R1 R2` |
| 7 | BR | Branch to address | `BR R1 R2` |
| 8 | SAL | Arithmetic left shift | `SAL R1 2` |
| 9 | SAR | Arithmetic right shift | `SAR R1 2` |
| 10 | LDR | Load from memory | `LDR R1 12` |
| 11 | STR | Store to memory | `STR R1 12` |

### Instruction Format

Each instruction is encoded as a 16-bit value:
- **Bits 15-12**: Opcode (4 bits)
- **Bits 11-6**: Source register (6 bits)
- **Bits 5-0**: Destination register/immediate/address (6 bits)

## 🚀 Installation

### Prerequisites

- GCC compiler (GNU Compiler Collection)
- Text editor for creating instruction files
- Windows/Linux/macOS

### Compilation

```bash
gcc -o cpu_simulator Main_File.c
```

## 📖 Usage

### 1. Create Instruction File

Create a text file named `data.txt` with your instructions:

```
ADD R1 R2
MOVI R3 10
SUB R1 R3
BEQZ R1 5
LDR R4 12
STR R1 15
```

### 2. Run the Simulator

```bash
./cpu_simulator
```

### 3. View Output

The simulator will display:
- Pipeline cycle information
- Register values before and after operations
- Memory operations
- Status register flags
- Program counter progression

## 📁 Project Structure

```
CA/
├── Main_File.c          # Main CPU simulator implementation
├── data.txt             # Instruction file (create this)
└── README.md           # This documentation
```

## 🔧 Technical Details

### Memory Layout

- **Instruction Memory**: 1024 × 16-bit words
- **Data Memory**: 2048 × 8-bit bytes
- **Register File**: 64 × 8-bit registers

### Register Usage

- **R0-R63**: General-purpose registers
- **R1-R6**: Pre-initialized with test values
- **Memory Location 12**: Pre-initialized with value 9

### Pipeline Implementation

The simulator implements a 3-stage pipeline:

1. **Fetch**: `fetch()` - Retrieves instruction from memory
2. **Decode**: `decode()` - Parses instruction fields
3. **Execute**: `execute()` - Performs operation

## 🚩 Status Register (SREG)

The 5-bit status register tracks operation results:

| Bit | Flag | Description |
|-----|------|-------------|
| 0 | Zero Flag | Set when result equals zero |
| 1 | Sign Flag | XOR of Negative and Overflow flags |
| 2 | Negative Flag | Set when result is negative |
| 3 | Overflow Flag | Set on arithmetic overflow |
| 4 | Carry Flag | Set on carry/borrow |

## 💾 Memory Layout

### Instruction Memory
- **Size**: 1024 instructions
- **Width**: 16 bits per instruction
- **Addressing**: 0-1023

### Data Memory
- **Size**: 2048 bytes
- **Width**: 8 bits per byte
- **Addressing**: 0-2047

### Register File
- **Size**: 64 registers
- **Width**: 8 bits per register
- **Addressing**: R0-R63

## 🔄 Pipeline Stages

### Stage 1: Fetch
```c
void fetch() {
    fetched = instructionMemory[pc++];
}
```

### Stage 2: Decode
```c
void decode() {
    opcode = instruction >> 12;
    rs = (instruction & 0b0000111111000000) >> 6;
    rt = (instruction & 0b0000000000111111);
}
```

### Stage 3: Execute
```c
void execute() {
    switch(opcode) {
        case 0: // ADD
        case 1: // SUB
        // ... other operations
    }
}
```

## 🧪 Testing

The simulator includes built-in test initialization:

```c
registerFile[1] = 2;
registerFile[2] = 4;
registerFile[3] = 3;
registerFile[4] = 4;
registerFile[5] = 6;
registerFile[6] = 1;
dataMemory[12] = 9;
```

## 🔍 Debugging Features

- Cycle-by-cycle execution display
- Register value tracking (before/after)
- Memory operation logging
- Status flag monitoring
- Pipeline stage visualization

## 📝 Example Output

```
cycle 1
PC : 0
fetch instruction 1 : 2048
There is no instruction neither decoded nor executed in this cycle
-----------------------------
cycle 2
PC : 1
fetch instruction 2 : 3072
decode instruction 1 : 2048
There is no instruction executed in this cycle
SREG :
Carry iS : 0
Overflow iS : 0
Negative bit iS : 0
Sign Flag iS : 0
Zero Flag iS : 0
-----------------------------
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 🙌 Author
**Momen H.**  
📂 [GitHub Profile »](https://github.com/Momenh2)




**Note**: This CPU simulator is designed for educational purposes and demonstrates fundamental computer architecture concepts. It implements a simplified but complete instruction set architecture with pipelined execution. 
