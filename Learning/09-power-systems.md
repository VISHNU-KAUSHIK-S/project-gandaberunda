# Project Gandaberunda — 09 Power Systems

Project Gandaberunda • Both developers complete every section

# Prerequisites

Electronics and LiPo fundamentals.

# What to learn

Power tree, current budget, voltage rails, regulators, buck converters, voltage drop, transient loads, brownouts, decoupling, grounding and power distribution.

# Explanation

## 1. Power tree

Electrical power is the rate of energy transfer: P = V × I. In the drone, power is critical because motors can consume far more power than the control electronics.

## 2. Current budget

Current is the rate at which electric charge flows. Propulsion can draw large current, while the STM32 and sensors usually require much smaller currents. Wiring, connectors and regulators must be selected for the current they actually carry.

## 3. Voltage rails

Voltage is the electrical potential difference that provides the push for charge to move. In Gandaberunda, every component has an allowed voltage range; a wrong rail can cause malfunction or permanent damage.

## 4. Regulators

Regulators is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Buck converters

Buck converters is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Voltage drop

Voltage is the electrical potential difference that provides the push for charge to move. In Gandaberunda, every component has an allowed voltage range; a wrong rail can cause malfunction or permanent damage.

## 7. Transient loads

Transient loads is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Brownouts

Brownouts is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. Decoupling

Decoupling is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Grounding

Grounding is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 11. Power distribution.

Electrical power is the rate of energy transfer: P = V × I. In the drone, power is critical because motors can consume far more power than the control electronics.

# How to study

Draw the V1 power tree and calculate expected loads. Measure regulator outputs under safe bench conditions. Study how propulsion transients and wiring resistance can disturb sensitive digital rails.

# Labs

1) Build a power budget. 2) Measure battery voltage. 3) Measure regulator input/output. 4) Measure safe bench-load voltage drop. 5) Check decoupling/grounding. 6) Record likely brownout causes.

# Gandaberunda connection

Battery power feeds ESCs and motors while regulated rails feed STM32, ESP32 and sensors. Power integrity must prevent resets and invalid sensor data.

# Must know

Power budget; current; voltage drop; regulator selection; brownout; decoupling; grounding; transient load.

# Pass test

Both developers can draw the power tree, calculate a basic current budget and explain three causes of brownout/reset.

# Deliverable

docs/learning/lab-notes/power-budget.md and power-test notes.
