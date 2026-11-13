# Hardware Reference – ATMEL AVR-USB uC-Evaluationboard (NTB-AVR-USB-uCB Rev. 2.2C)

Source: `doc/avrusbexpboard_schema.pdf`. MCU: AT90USB1287, 8 MHz crystal (Y1).

## LEDs (active high: pin → 470 Ω → LED → GND)

| LED  | Pin |
|------|-----|
| LED1 | PD4 |
| LED2 | PD5 |
| LED3 | PD6 |
| LED4 | PD7 |

Set the pin as output in `DDRD`, switch on with `PORTD |= (1 << PD4)`.

## Push buttons (active low: 47 kΩ external pull-up, button to GND)

| Button | Pin | Note |
|--------|-----|------|
| S1     | PE2 | HWB – held at reset = start USB bootloader |
| S2     | –   | Reset |
| S3     | PE0 | |
| S4     | PE1 | |
| S5     | PE3 | marked optional on schematic (UID) |
| S6     | PE4 | INT4, shared with joystick RIGHT |
| S7     | PE6 | INT6 |
| S8     | PE5 | INT5, shared with joystick DOWN |

Pressed = pin reads `0`.

## Joystick U6 (active low, COM to GND → enable internal pull-ups on PB5..PB7)

| Direction | Pin |
|-----------|-----|
| UP        | PB7 |
| DOWN      | PE5 |
| LEFT      | PB6 |
| RIGHT     | PE4 |
| SELECT    | PB5 |

## Analog inputs (only active if the solder pad is closed)

| Function             | Pin       | Pad |
|----------------------|-----------|-----|
| NTC temperature R36  | PF0/ADC0  | P8  |
| Potentiometer R7     | PF1/ADC1  | P9  |
| Microphone (amp.)    | PF2/ADC2  | P10 |
| Light sensor LDR L2  | PF3/ADC3  | P7  |

PF4..PF7 are JTAG (TCK/TMS/TDO/TDI); usable as GPIO only with JTAG disabled (JTD bit in MCUCR, write twice).

## Other peripherals

| Function                     | Pins                          |
|------------------------------|-------------------------------|
| RS232 (MAX3232) USART1       | PD2 = RXD1, PD3 = TXD1        |
| RS232 handshake (pads P1–P4) | PD0 = RTS, PD1 = CTS          |
| I2C / TWI (display EA T123A) | PD0 = SCL, PD1 = SDA (10 kΩ pull-ups) – not together with handshake |
| Buzzer LS1 (via BC846B)      | PC6 (OC3A, Timer3 PWM)        |
| SPI Data Flash M25P32        | PB1 SCK, PB2 MOSI, PB3 MISO, PB4 /CS – only with 3.3 V regulator U4 |
| ISP connector JP1            | PB1, PB2, PB3, /RESET         |
| Extension connectors         | J4 (PA, PB0/5–7, PC), J5 (PF), CON26 (PD, PE, PF, AVCC, AREF) |

## Datasheet chapters (`doc/at90usb64x_at90usb128x_userguide.pdf`, page = PDF page)

| Topic                              | Chapter | Page |
|------------------------------------|---------|------|
| Pin configuration / descriptions   | 1, 2.2  | 3, 7 |
| Clock, system clock prescaler      | 6, 6.9  | 39, 47 |
| Power management / sleep           | 7       | 51 |
| Reset, watchdog                    | 8       | 57 |
| Interrupt vectors                  | 9       | 68 |
| I/O ports (DDRx, PORTx, PINx)      | 10      | 72 |
| Port register description          | 10.4    | 90 |
| External interrupts INTx / PCINT   | 11      | 94 |
| Timer prescalers                   | 12      | 98 |
| Timer0 (8 bit, PWM)                | 13      | 100 |
| Timer1 / Timer3 (16 bit)           | 14      | 117 |
| Timer2 (8 bit, async)              | 15      | 149 |
| SPI                                | 17      | 172 |
| USART                              | 18      | 181 |
| Baud rate tables                   | 18.10   | 203 |
| TWI (I2C)                          | 20      | 216 |
| USB controller                     | 21      | 246 |
| Analog comparator                  | 24      | 310 |
| ADC                                | 25      | 313 |
| Boot loader / self-programming     | 28      | 349 |
| Electrical characteristics         | 30      | 398 |
| Register summary                   | 32      | 427 |
| Errata AT90USB1287                 | 36      | 442 |
