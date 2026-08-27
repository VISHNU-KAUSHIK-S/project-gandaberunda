# Project Gandaberunda — 15 UART

Project Gandaberunda • Both developers complete every section

# Prerequisites

Embedded systems and STM32 basics.

# What to learn

UART TX/RX, baud rate, frame structure, blocking vs non-blocking, interrupts, buffers, DMA, timeouts, packet framing and error handling.

# Explanation

## 1. Uart tx/rx

UART is an asynchronous serial interface using TX and RX. In V1 it carries validated command data between the drone ESP32 and STM32.

## 2. Baud rate

Baud rate is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Frame structure

Frame structure is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Blocking vs non-blocking

Blocking vs non-blocking is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Interrupts

An interrupt lets hardware request CPU attention when an event occurs. This avoids constantly polling for events and can make communication and timing handling more responsive.

## 6. Buffers

Buffers is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Dma

DMA lets compatible peripherals move data to or from memory with less CPU involvement. This can reduce processing overhead for repeated data transfers.

## 8. Timeouts

Timeouts is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. Packet framing

Packet framing is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Error handling.

Error is the difference between the desired value and measured value: error = setpoint − measurement. The controller uses this error to determine the correction required.

# How to study

Learn UART electrically and in firmware. Start with raw bytes, then build a structured packet. Measure communication timing and learn how the receiver distinguishes valid, fresh commands from invalid or stale data.

# Labs

1) STM32 transmit. 2) STM32 receive. 3) ESP32 ↔ STM32 communication. 4) Interrupt-based receive. 5) Buffering. 6) Packet timeout. 7) Safe invalid-packet test. 8) Optional DMA test.

# Gandaberunda connection

V1 uses UART between the drone ESP32 receiver and STM32. UART transports commands; the STM32 remains responsible for flight control and safety.

# Must know

TX/RX; baud rate; frames; buffers; interrupts; DMA; packet framing; timeout; error handling.

# Pass test

Both developers can configure UART, explain packet timing and explain what the STM32 should do with missing, stale or invalid data.

# Deliverable

docs/learning/lab-notes/uart.md
