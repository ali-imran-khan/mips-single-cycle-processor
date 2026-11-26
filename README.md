# mips-single-cycle-processor

A single-cycle MIPS subset written in VHDL and run on a DE1-SoC (Cyclone V, 5CSEMA5F31C6). Built for the Computer Organization & Architecture lab (CPE-343) at COMSATS Lahore.

```
fetch → decode (register file + sign extend) → execute (ALU, branch/jump target) → data memory → write back
                       ↑
                    control (opcode → RegDst, ALUSrc, MemtoReg, RegWrite, MemRead, MemWrite, Branch, Jump, ALUOp)
```

Supported instructions: R-type `add`, `sub`, `nand`, plus `lw`, `sw`, `beq`, `j`, `addi`.

| file | |
|---|---|
| `src/fetch.vhd` | PC + 16-word instruction ROM (the test program is hardcoded here) |
| `src/decode.vhd` | register file, sign extension |
| `src/control.vhd` | main control unit |
| `src/execute.vhd` | ALU, branch decision, branch/jump address |
| `src/datamemory.vhd` | data RAM |
| `src/my_wrapper.vhd` | top level, wires the stages together and picks what goes on the seven-segment displays |
| `src/SevenSegment.vhd` | hex → 7-seg decoder |
| `sim/` | Quartus waveform files used to check each stage |

## Running it

Open `fetch.qpf` in Quartus Prime (top-level entity is `my_wrapper`), compile, and program the DE1-SoC. The clock is `KEY0`, so you step one instruction per press, and reset is `SW9`. The 3-bit `opt` input picks which internal value (PC, instruction, ALU result, register values…) is shown in hex on HEX0–HEX5. That's how we debugged it on the board. Check the pin assignments for `opt` in the `.qsf` before compiling, because a few are left over from earlier stages.

To run a different program, replace the hex words in `instruction_memory` inside `fetch.vhd`. There's a second test program commented out right below the active one.
