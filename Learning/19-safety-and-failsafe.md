# Project Gandaberunda — 19 Safety & Failsafe

Project Gandaberunda • Both developers complete every section

# Prerequisites

Full system architecture and communication/control fundamentals.

# What to learn

Arming, disarming, pre-arm checks, communication loss, sensor failure, battery monitoring, watchdog/reset, emergency control, state transitions and safe fault handling.

# Explanation

## 1. Arming

Arming is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Disarming

Disarming is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Pre-arm checks

Pre-arm checks is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Communication loss

Communication loss is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Sensor failure

Sensor failure is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Battery monitoring

Battery monitoring is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Watchdog/reset

Watchdog/reset is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Emergency control

Emergency control is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. State transitions

State transitions is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Safe fault handling.

Safe fault handling. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Treat safety as a system state machine. Define what conditions are required before arming and what happens when a condition becomes invalid. Test each response on the bench before flight.

# Labs

1) Draw arm/disarm state machine. 2) Test pre-arm conditions. 3) Test communication timeout. 4) Test invalid sensor condition. 5) Test watchdog recovery. 6) Verify no automatic re-arm after reset/recovery.

# Gandaberunda connection

The STM32 is the final authority. It must reject invalid commands, apply failsafe behaviour and prevent unintended motor operation.

# Must know

Pre-arm checks; arm/disarm; timeout; sensor validation; watchdog; reset recovery; safe state transitions.

# Pass test

Both developers can explain at least three failure scenarios and the expected safe response without relying on the other developer.

# Deliverable

docs/learning/lab-notes/failsafe-tests.md
