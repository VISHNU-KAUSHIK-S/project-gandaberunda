# Project Gandaberunda — 17 ESP-NOW

Project Gandaberunda • Both developers complete every section

# Prerequisites

ESP32 basics and communication fundamentals.

# What to learn

ESP-NOW peer communication, packet structure, addressing, delivery status, latency, packet loss, sequence numbers, freshness and safe command handling.

# Explanation

## 1. Esp-now peer communication

ESP-NOW is the wireless ESP32-to-ESP32 link used in V1. It transports controller packets; it does not replace the STM32's flight-control authority.

## 2. Packet structure

Packet structure is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Addressing

Addressing is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Delivery status

Delivery status is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Latency

Latency is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Packet loss

Packet loss is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Sequence numbers

Sequence numbers is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Freshness

Freshness is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. Safe command handling.

Safe command handling. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

First establish a reliable ESP32-to-ESP32 link. Then add a compact command packet with validation and freshness information. Measure the link before using it for drone commands.

# Labs

1) Pair ESP32 devices. 2) Send test packets. 3) Add structured commands. 4) Add sequence number. 5) Measure latency/loss. 6) Test stale/invalid packets safely. 7) Forward validated data over UART.

# Gandaberunda connection

Controller ESP32 → ESP-NOW → drone ESP32 is the wireless V1 command link. The receiver forwards data to the STM32; it does not directly control motors.

# Must know

Peer setup; packet structure; delivery status; sequence; freshness; latency; loss; receiver behaviour.

# Pass test

Both developers can explain the complete wireless path, packet validation and what happens when the link becomes unavailable.

# Deliverable

docs/learning/lab-notes/esp-now.md
