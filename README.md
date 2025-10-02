# Embedded Programming with C8051F040 Microcontroller
This repository contains two assembly projects developed for the Silicon Labs C8051F040 microcontroller. Both programs are written in low-level Assembly and demonstrate direct register manipulation, peripheral initialization, and digital control via GPIO and UART interfaces.

# Project 1: LED Bar Controller with ADC
This program implements a constant-time infinite loop that controls two LEDs (red and green) based on the analog voltage read from a solar cell.

#The algorithm works in three main steps:

1. The solar cell voltage is converted into an 8-bit digital value (0–255).

2. This value is divided by 16, resulting in a number between 0 and 15, which determines the "color level."

3. The red and green LEDs are turned on for specific time intervals (t1 and t2), depending on this color level.

Since the loop duration is constant, the sum of the red and green LED intervals (T = t1 + t2) remains fixed, divided into 16 equal time quanta.

- Subroutine lightTest performs the analog-to-digital conversion. The result sets the number of time quanta for the red LED (stored in register R6). The green LED time is then calculated as 15 - R6 (stored in R7).

- Subroutine baseDelay introduces a delay based on counter register R2.

- Main routine initializes the system and enters the infinite loop. It calls lightTest, then lights up the red LED for R6 * baseDelay quanta and the green LED for R7 * baseDelay quanta, ensuring the total cycle time stays constant.

# Features:
- 10 LEDs controlled individually (LED1 to LED10)
- Uses ADC2 to convert analog input
- LED bar visualizes the intensity level
- Fully interrupt-based and efficient looped execution

# Project 2: UART Character Sequence Transmitter
This program implements an infinite loop (labeled start) that processes characters received via UART1 and transmits a sequence of ASCII codes to the terminal (e.g., PuTTY).

#The algorithm works as follows:

1. Receive input: The subroutine receiveByte receives one byte and stores it in the accumulator.

2. Validation: The received byte is compared to the ASCII codes of characters A and Z. If the value is outside this range, the program jumps to the error routine.

3. Store and prepare transmission:

- The valid character is stored in register R0.

- Register R1 is initialized with the number of characters to send.

- The loop loop sends consecutive ASCII characters starting from the received one up to Z. Each iteration calls sendByte, increments R0, and decrements R1 until all characters are sent.

4. Error handling: If the received character is invalid, the error routine sends three "?" characters instead.

The main routine initializes the system, then continuously calls this process in an infinite loop.

#Subroutines

- sendByte: Sends the ASCII code stored in R0 via UART1 by writing to SBUF1, waits for the transmission flag (TI1), then resets it.

- receiveByte: Waits for the reception flag (RI1), retrieves the byte from SBUF1, stores it in the accumulator, and resets the flag.

- init: Handles system setup, including disabling interrupts and watchdog, enabling the crossbar and UART1, selecting the system clock (24.5 MHz internal oscillator), configuring Timer 1 for baud rate generation (115200 bps, 8-bit auto-reload), and setting UART1 in 8-bit mode with reception enabled.

# Features:
- UART1 communication via polling
- Character validation
- Response with incremental character sequence
- Error handling and looped execution

# Tools & Technologies
- Microcontroller: Silicon Labs C8051F040
- Language: Assembly (8051-compatible)
- IDE: Silicon Labs IDE / Keil µVision / Any 8051-compatible assembler
- Architecture: 8051 core
