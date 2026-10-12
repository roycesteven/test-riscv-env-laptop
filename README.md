# RISC-V memory-safety exercise (MSc coursework)

Coursework from the MSc in Cyber Security at the University of Manchester,
December 2025. The course staff supplied the template, the sixteen bug
programs and the devcontainer (commits by Pierre Olivier and Yeshwanth,
September to December 2025). My commits are the ones authored by Royce
Steven, 5 to 9 December 2025.

The companion repository
[test-cheri-env-laptop](https://github.com/roycesteven/test-cheri-env-laptop)
runs the same programs as compartments on the CHERIoT capability
architecture.

## What is here

Eight memory-safety bug classes, each written in C (`exercises-c/`) and
C++ (`exercises-c++/`):

| # | Bug class |
|---|---|
| 01 | heap buffer over-read |
| 02 | heap buffer over-write |
| 03 | heap buffer under-write |
| 04 | stack buffer over-write |
| 05 | out-of-bounds pointer arithmetic |
| 06 | use-after-free |
| 07 | double free |
| 08 | type confusion |

Each program is built for 32-bit RISC-V (`-march=rv32g -mabi=ilp32`) with
the `riscv32-unknown-elf` GCC toolchain through xmake, and runs with
semihosting on Google's
[MPACT-RiscV](https://github.com/google/mpact-riscv) instruction-set
simulator (`mpact_rv32g_sim`).

## My part

- Added debug symbols (`-g`) to the toolchain definition in all 16
  `xmake.lua` files.
- Ran every program on the simulator. Each run writes the simulator's
  statistics, including per-instruction-type execution counts and the total
  instruction count, to
  `submit-riscv/<language>/<exercise>/runs/run_<timestamp>/<program>.proto`.
  The `debug` file next to `runs/` records the times of interactive debug
  runs.
- Disassembled every binary and recorded each program's crash address in
  [`riscv-crash-addresses.csv`](riscv-crash-addresses.csv) (16 entries).
  The disassembly listings are in the history at commit `7455901`.

## Running an exercise

Open the repository in its devcontainer (`.devcontainer/`), which provides
the toolchain and the simulator, then from an exercise directory:

```sh
xmake        # build the rv32g binary
xmake run    # run it on mpact_rv32g_sim; statistics go to a new run directory under submit-riscv/
```
