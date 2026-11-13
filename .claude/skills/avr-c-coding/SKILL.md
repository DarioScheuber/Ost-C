---
name: avr-c-coding
description: Coding rules for the AVR C projects in this workspace (AT90USB1287 on the OST AVR-USB evaluation board, avr-gcc, Makefile template). Use whenever writing, changing or reviewing C code (.c/.h) in any 0x_AVR_Project_* folder, or when a task needs board pins (LEDs, switches, joystick, ADC, UART, buzzer) or microcontroller registers.
---

# AVR C Coding (AT90USB1287)

## Target
- MCU: **AT90USB1287** (`MCU = at90usb1287` in the Makefile), 8 MHz crystal, CKDIV8 → `F_CPU 1000000UL`.
- Language: **C only** (gnu99, avr-gcc / WinAVR, avr-libc). No C++, no Arduino API.
- Board pinout: see [hardware.md](hardware.md). Always check it before using a pin.
- Sources (workspace folder `doc/`):
  - Schematic: `doc/avrusbexpboard_schema.pdf`
  - Datasheet / user manual: `doc/at90usb64x_at90usb128x_userguide.pdf` (chapter pages in hardware.md)
  - Take register names, bits and timing from the datasheet, do not guess.

## Project structure
| Path            | Content                                                              |
|-----------------|----------------------------------------------------------------------|
| `app/main.c`    | application / test program                                           |
| `src/`          | own C modules / drivers (one module per peripheral, e.g. `led.c`)    |
| `includes/`     | own public header files, one per module, with include guard          |
| `ext_includes/` | header files of external libraries (do not edit)                     |
| `ext_libs/`     | external static libraries `lib<name>.a`, linked automatically as `-l<name>` |
| `Makefile`      | build targets                                                        |

- New `.c` files in `src/` and `.a` files in `ext_libs/` are picked up by the Makefile automatically, no Makefile change needed.
- Generated folders (`build/`, `bin/`, `dist/`) are never edited by hand.
- Build in the project folder: `make compile`, `make build`, `make flash`, `make clean`;
  `make pack_lib LIBNAME=<name> VERSION=<vXX>` packs `src/` + `includes/` as a library into `dist/`.

## Code style
- `F_CPU` defined before `#include <util/delay.h>`.
- Use `<stdint.h>` types (`uint8_t`, `uint16_t`, ...), not `int`/`char` for register data.
- Bit access with `(1 << PD4)` and read-modify-write (`|=`, `&= ~`), never overwrite whole ports by accident.
- Pin assignments as `#define` in the module header, not as magic numbers in the code.
- Variables shared with an ISR are `volatile`; keep ISRs short.
- No `malloc`, no float `printf`, avoid floats where integer math works.
- Formatting: LLVM style, 4 spaces, braces attached (as in `.vscode/settings.json`).

## Naming
Everything in `snake_case`. Variables and parameters start with a type prefix:

| Prefix | Type                                      | Example                  |
|--------|-------------------------------------------|--------------------------|
| `n_`   | integer (`uint8_t`, `int16_t`, `int`, ...) | `uint8_t n_led_count`    |
| `f_`   | `float` / `double`                         | `float f_voltage`        |
| `b_`   | `bool`                                     | `bool b_pressed`         |
| `c_`   | `char`                                     | `char c_rx`              |
| `s_`   | string (`char[]` / `char *` text)          | `char s_name[16]`        |
| `p_`   | pointer, followed by the target prefix     | `uint8_t *p_n_buffer`    |

- Temporary variables (loop counters, intermediate values, local scope only) get an additional leading `_`:
  `for (uint8_t _n_i = 0; _n_i < 4; _n_i++)`, `uint16_t _n_tmp = ADC;`
- Functions: module prefix, no type prefix (`led_init`, `adc_read`).
- Macros, constants and enum values: `UPPER_CASE` (`LED1_PIN`, `ADC_CH_POT`).
- Types: `snake_case` with `_t` suffix (`led_state_t`).

## Comments
Keep comments short. Comment the *why*, not the *what*. No line-by-line narration, no commented-out code, no empty section banners.

File header (every .c / .h):

```c
/*******************************************************************************
 * Project : <project folder name>
 * File    : <file name>
 * Description   : <one line>
 * Target  : AT90USB1287 @ 1 MHz
 * Author  : <name> | OST
 * Date    : <dd.mm.yyyy>
 ******************************************************************************/
```

Every function definition gets this short header (In = parameters, Out = return value, `-` if none):

```c
//------------------------------------------------------------------------------
// Description: <Description>
// In:    <param> - <meaning, range/unit>
// Out:   <return value meaning>
//------------------------------------------------------------------------------
```

Example:

```c
//------------------------------------------------------------------------------
// Description: Switches one LED on.
// In:    n_led - LED number 1..4
// Out:   -
//------------------------------------------------------------------------------
void led_on(uint8_t n_led) {
    ...
}
```

- `main()` gets the same header.
- Prototypes in headers do not repeat the header block.
- Inside a function only a comment where the code is not self-explanatory (e.g. why a register bit is set).

## Before finishing
- Check pin and register names against hardware.md / the datasheet.
- Run `make compile` in the project folder when the toolchain is available, fix all warnings.
