# Project Gandaberunda — 22 Debugging

Project Gandaberunda • Both developers complete every section

# Prerequisites

Embedded systems, electronics and testing.

# What to learn

Symptom isolation, reproduce/observe/hypothesize/test, multimeter use, serial logs, logic analyzer/oscilloscope use, firmware debugging, power checks and root-cause analysis.

# Explanation

## 1. Symptom isolation

Symptom isolation is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Reproduce/observe/hypothesize/test

Reproduce/observe/hypothesize/test is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Multimeter use

Multimeter use is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Serial logs

Serial logs is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Logic analyzer/oscilloscope use

Logic analyzer/oscilloscope use is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Firmware debugging

Firmware debugging is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Power checks

Electrical power is the rate of energy transfer: P = V × I. In the drone, power is critical because motors can consume far more power than the control electronics.

## 8. Root-cause analysis.

Root-cause analysis. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Debug from the simplest boundary outward: power, wiring, signal, peripheral, firmware, data and system state. Change one variable at a time and record what the evidence says.

# Labs

1) Debug a GPIO fault. 2) Debug a UART fault. 3) Debug a sensor reading fault. 4) Debug a timing issue. 5) Debug a power/reset symptom on a safe bench. 6) Write root-cause reports.

# Gandaberunda connection

Gandaberunda will combine many interacting subsystems, so debugging must identify whether a problem is electrical, firmware, communication, estimation, control or mechanical before changing the design.

# Must know

Reproduction; isolation; evidence; instrumentation; root cause; one-change-at-a-time testing; regression check.

# Pass test

Both developers can take an unknown bench failure, produce a testable hypothesis, gather evidence, identify the root cause and verify the fix.

# Deliverable

docs/learning/lab-notes/debugging-reports/
