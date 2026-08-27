# Project Gandaberunda — 06 ESC & Motor Control

Project Gandaberunda • Both developers complete every section

# Prerequisites

BLDC motors, PWM and embedded systems.

# What to learn

ESC purpose, BLDC commutation, control input, PWM command, arming behaviour, throttle mapping, calibration/limits, current handling and later DShot concepts.

# Explanation

## 1. Esc purpose

Esc purpose is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Bldc commutation

Bldc commutation is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Control input

Control input is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Pwm command

PWM is a timed digital waveform whose timing characteristics encode a command. In V1 the STM32 uses PWM to command ESCs.

## 5. Arming behaviour

Arming behaviour is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Throttle mapping

Throttle mapping is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Calibration/limits

Calibration/limits is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Current handling

Current is the rate at which electric charge flows. Propulsion can draw large current, while the STM32 and sensors usually require much smaller currents. Wiring, connectors and regulators must be selected for the current they actually carry.

## 9. Later dshot concepts.

Later dshot concepts. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

First understand the ESC as the interface between STM32 and the BLDC motor. Learn what the command signal means and how the ESC converts it into phase drive. Test only without propellers during bench learning.

# Labs

1) Identify ESC power/motor/signal connections. 2) Generate a safe PWM signal. 3) Verify signal timing. 4) Test ESC response with propellers removed. 5) Verify minimum/maximum command behaviour. 6) Record failure observations safely.

# Gandaberunda connection

STM32 → PWM V1 → ESCs → BLDC motors. DShot is a later upgrade boundary, not a V1 requirement.

# Must know

ESC role; signal vs power; PWM command; arming; motor direction; limits; safe bench testing.

# Pass test

Both developers can trace the complete command path from STM32 PWM to ESC to motor and explain the safe procedure for bench testing.

# Deliverable

docs/learning/lab-notes/esc-motor-test.md
