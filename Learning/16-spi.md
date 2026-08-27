# Project Gandaberunda — 16 SPI

Project Gandaberunda • Both developers complete every section

# Prerequisites

Embedded systems and STM32 basics.

# What to learn

SPI master/slave concept, SCLK, MOSI, MISO, CS, modes, full-duplex transfer, timing, sensor register access and comparison with UART/I2C.

# Explanation

## 1. Spi master/slave concept

SPI is a synchronous serial bus using a clock and data lines. It is useful for fast peripheral/register access and may be evaluated for future interfaces.

## 2. Sclk

Sclk is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Mosi

Mosi is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Miso

Miso is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Cs

Cs is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Modes

Modes is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Full-duplex transfer

Full-duplex transfer is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Timing

Timing is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. Sensor register access

Sensor register access is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Comparison with uart/i2c.

UART is an asynchronous serial interface using TX and RX. In V1 it carries validated command data between the drone ESP32 and STM32.

# How to study

Understand SPI as a synchronous bus controlled by a clock. Connect a simple peripheral, read a register and verify the transaction with a logic analyzer if available. Evaluate SPI for future Gandaberunda interfaces rather than assuming it is automatically better.

# Labs

1) Identify SPI signals. 2) Configure STM32 SPI. 3) Read/write a peripheral register. 4) Measure clock and chip-select timing. 5) Compare SPI and UART for a chosen V1 use case.

# Gandaberunda connection

SPI may be used for sensor/peripheral interfaces and may replace or supplement selected V1 interfaces only after testing demonstrates a benefit.

# Must know

SCLK; MOSI; MISO; CS; SPI modes; transaction timing; register reads/writes; bus selection.

# Pass test

Both developers can draw an SPI transaction, explain every signal and read a basic peripheral register correctly.

# Deliverable

docs/learning/lab-notes/spi.md
