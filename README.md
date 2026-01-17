## Printf function implementation via OpenOCD and semi-hosting on STM32

# This project was created to show how to implement printf function via OpenOCD and semi-hosting on STM32 microcontroller.

---

# What is the semi-hosting debug method?
The semi-hosting debug method is designed for use host resources. 
Semi-hosting is only for debugging and education, it never uses in production.

This method allows:
- Print the text into debug console via printf(), puts() etc. functions
- read keypad input
- work with file on host (open, read, write)
- get current host time and date
- another system calls

---

# How it works:
- printf() calls _write() function
- _write() makes semi-hosting call (BKPT 0xAB)
- OpenOCD intercepts this call 
- Text viewed on host console

---

# Used technologies:
- Microcontroller: STM32F407VET6
- IDE: STM32CubeIDE / CLion
- Programming language: C
- Programmer: ST-LINK-V2.1 (STM32CubeProgrammer)
- Debug: arm-none-eabi-gdb, arm-none-eabi-gcc
- OpenOCD ver. 0.12.0

---

# Compilation and linkage
Linkage with flag: -specs=rdimon.specs -lc -lrdimon

---

# GDB:

```gdb
target remote: 3333
monitor reset halt
continue
```

# Warnings
- semi-hosting slows the program
- requires a debugger
- creates breakpoints on every printf() and stops the core on every call
- For the real projects you must use UART/USART or ITM (SWO). This method is only for education or debugging.