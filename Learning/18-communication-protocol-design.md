# Project Gandaberunda — 18 Communication Protocol Design

Project Gandaberunda • Both developers complete every section

# Prerequisites

UART, ESP-NOW and embedded systems.

# What to learn

Packet framing, message types, fields, sequence numbers, timestamps/freshness, CRC/checksum, ranges, versioning, timeouts and protocol state.

# Explanation

## 1. Packet framing

Packet framing is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Message types

Message types is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Fields

Fields is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Sequence numbers

Sequence numbers is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Timestamps/freshness

Timestamps/freshness is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Crc/checksum

Crc/checksum is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Ranges

Ranges is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Versioning

Versioning is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. Timeouts

Timeouts is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Protocol state.

Protocol state. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Design the protocol from system requirements. Define every field and its units. Decide how invalid, duplicated, stale and out-of-range packets are handled. Keep the protocol small enough to debug and evolve.

# Labs

1) Write packet requirements. 2) Define fields. 3) Implement encode/decode. 4) Add sequence checking. 5) Add CRC/checksum. 6) Inject safe invalid packets. 7) Measure latency and timeout behaviour.

# Gandaberunda connection

The protocol carries pilot commands from the controller to the STM32 through the two-stage ESP32 link. Safety logic is performed by the STM32 before motor commands are generated.

# Must know

Framing; sequence; freshness; validation; CRC/checksum; versioning; timeouts; command ranges.

# Pass test

Both developers can draw the V1 packet and explain what happens for valid, missing, duplicated, stale, corrupted and out-of-range packets.

# Deliverable

docs/learning/lab-notes/communication-protocol.md
