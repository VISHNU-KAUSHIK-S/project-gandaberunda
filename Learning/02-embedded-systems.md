# Project Gandaberunda — 02 Embedded Systems

Project Gandaberunda • Both developers complete every section

# Prerequisites

Electronics fundamentals.

# What to learn

MCU vs CPU, Flash, RAM, GPIO, registers, timers, interrupts, polling, ADC, PWM, UART, I2C, SPI, DMA, watchdogs, clocks, reset, boot, deterministic timing and real-time constraints.

# Explanation

## 1. Mcu vs cpu

Mcu vs cpu is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Flash

Flash is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Ram

Ram is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Gpio

GPIO pins are configurable digital inputs or outputs. Firmware changes peripheral configuration and the physical pin then reads or drives a digital signal.

## 5. Registers

Registers are hardware-controlled locations used to configure peripherals and read their state. Understanding the register/peripheral relationship helps explain what embedded C code is actually doing.

## 6. Timers

Timers count clock ticks in hardware and can generate precise events or waveforms. They are important for PWM and predictable flight-loop timing.

## 7. Interrupts

An interrupt lets hardware request CPU attention when an event occurs. This avoids constantly polling for events and can make communication and timing handling more responsive.

## 8. Polling

Polling means software repeatedly checks whether an event has happened. It is simple but can waste CPU time and make timing less efficient when many events must be handled.

## 9. Adc

Adc is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Pwm

PWM is a timed digital waveform whose timing characteristics encode a command. In V1 the STM32 uses PWM to command ESCs.

## 11. Uart

UART is an asynchronous serial interface using TX and RX. In V1 it carries validated command data between the drone ESP32 and STM32.

## 12. I2c

I2c is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 13. Spi

SPI is a synchronous serial bus using a clock and data lines. It is useful for fast peripheral/register access and may be evaluated for future interfaces.

## 14. Dma

DMA lets compatible peripherals move data to or from memory with less CPU involvement. This can reduce processing overhead for repeated data transfers.

## 15. Watchdogs

A watchdog monitors firmware progress and can reset the MCU if software stops servicing it. It is a safety mechanism, not a replacement for correct software.

## 16. Clocks

Clocks is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 17. Reset

Reset is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 18. Boot

Boot is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 19. Deterministic timing

Deterministic timing is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 20. Real-time constraints.

Real-time constraints. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Start with a microcontroller block diagram. Follow one GPIO from software register to physical pin. Compare polling with interrupts. Learn timers and then study why a flight controller needs predictable loop timing.

# Labs

1) Blink an LED without blocking delays. 2) Generate a periodic timer event. 3) Read a button using GPIO. 4) Measure timing with a logic analyzer/oscilloscope if available. 5) Send/receive serial data. 6) Trigger and recover from a watchdog reset.

# Gandaberunda connection

The STM32 must repeatedly execute sensor acquisition → estimation → control → motor output while handling communication and safety.

# Must know

Interrupt vs polling; DMA; watchdog operation; timing jitter; real-time constraints.

# Pass test

Both developers can draw the MCU execution flow and explain what happens when an interrupt, UART packet or watchdog event occurs.

# Deliverable

docs/learning/lab-notes/embedded-lab-report.md and timing notes.
