# Project Gandaberunda — 01 Electronics

Project Gandaberunda • Both developers complete every section

# Prerequisites

None. Start from voltage/current/resistance.

# What to learn

Voltage, current, resistance, Ohm’s law, power, energy, series/parallel circuits, Kirchhoff laws, ground, voltage rails, digital/analog signals, logic levels, capacitors, inductors, diodes, MOSFETs, regulators, buck converters, connectors, wire resistance, fuses and electrical noise.

# Explanation

## 1. Voltage

Voltage is the electrical potential difference that provides the push for charge to move. In Gandaberunda, every component has an allowed voltage range; a wrong rail can cause malfunction or permanent damage.

## 2. Current

Current is the rate at which electric charge flows. Propulsion can draw large current, while the STM32 and sensors usually require much smaller currents. Wiring, connectors and regulators must be selected for the current they actually carry.

## 3. Resistance

Resistance opposes current. It affects voltage drop, heating and signal behaviour. Wire and connector resistance matter when high propulsion current flows.

## 4. Ohm’s law

Ohm’s law relates voltage, current and resistance: V = I × R. It lets you predict current or voltage drop in simple circuits and check whether a measurement makes sense.

## 5. Power

Electrical power is the rate of energy transfer: P = V × I. In the drone, power is critical because motors can consume far more power than the control electronics.

## 6. Energy

Energy describes how much work a battery can provide over time. A battery's usable energy helps determine expected endurance, while the propulsion load determines how quickly that energy is consumed.

## 7. Series/parallel circuits

Series/parallel circuits is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Kirchhoff laws

Kirchhoff laws is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. Ground

Ground is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Voltage rails

Voltage is the electrical potential difference that provides the push for charge to move. In Gandaberunda, every component has an allowed voltage range; a wrong rail can cause malfunction or permanent damage.

## 11. Digital/analog signals

Digital/analog signals is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 12. Logic levels

Logic levels is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 13. Capacitors

Capacitors is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 14. Inductors

Inductors is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 15. Diodes

Diodes is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 16. Mosfets

Mosfets is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 17. Regulators

Regulators is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 18. Buck converters

Buck converters is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 19. Connectors

Connectors is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 20. Wire resistance

Resistance opposes current. It affects voltage drop, heating and signal behaviour. Wire and connector resistance matter when high propulsion current flows.

## 21. Fuses

Fuses is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 22. Electrical noise.

Electrical noise. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Start with voltage, current and resistance. Solve V=IR and P=VI problems before moving to circuits. Draw the power and signal paths by hand. Read real module schematics and datasheets. Use safe low-voltage experiments to connect theory to measurements.

# Labs

1) Measure a battery voltage with a multimeter. 2) Measure resistor values. 3) Verify continuity. 4) Build a low-voltage resistor circuit. 5) Measure regulator input/output. 6) Diagnose a safe wrong connection on a low-voltage bench setup.

# Gandaberunda connection

Battery → distribution → ESCs and regulated electronics. Trace power separately from data signals.

# Must know

V=IR; P=VI; voltage vs current vs power; common ground; regulator purpose; voltage drop; brownouts; decoupling.

# Pass test

Both developers can read an unfamiliar wiring diagram, identify power/ground/signal, measure it safely and explain three likely electrical failure modes.

# Deliverable

docs/learning/lab-notes/electronics-basics.md
