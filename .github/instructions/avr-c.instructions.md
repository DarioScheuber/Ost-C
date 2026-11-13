---
applyTo: "**/*.c,**/*.h,**/Makefile"
description: AVR C coding rules for the AT90USB1287 projects in this workspace
---

Before writing or changing C code, read and follow the workspace skill **avr-c-coding**:

- Rules: [SKILL.md](../../.claude/skills/avr-c-coding/SKILL.md)
- Board pinout and datasheet chapters: [hardware.md](../../.claude/skills/avr-c-coding/hardware.md)

Short version: C only (avr-gcc, AT90USB1287 @ 1 MHz), `snake_case` with type prefixes
(`n_` int, `f_` float, `b_` bool, `c_` char, `s_` string, `p_` pointer, `_` for temporary
variables), every function gets a `//---` header with Description / In / Out, few comments in the code.

Project structure (each `0x_AVR_Project_*` folder):

- `app/main.c` → application program
- `src/` → own C modules / drivers
- `includes/` → own public header files
- `ext_includes/` → header files of external libraries
- `ext_libs/` → external static libraries (`lib<name>.a`)
- `Makefile` → build targets (`make compile`, `make build`, `make flash`, `make clean`, `make pack_lib`)
