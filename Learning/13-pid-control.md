# Project Gandaberunda — 13 PID Control

Project Gandaberunda • Both developers complete every section

# Prerequisites

Control systems, sensor estimation and flight dynamics.

# What to learn

Proportional, integral, derivative, error, gains, tuning, overshoot, settling, noise, integral windup, output limits and axis-specific control.

# Explanation

## 1. Proportional

Proportional is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Integral

Integral is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Derivative

Derivative is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Error

Error is the difference between the desired value and measured value: error = setpoint − measurement. The controller uses this error to determine the correction required.

## 5. Gains

Gains is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Tuning

Tuning is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Overshoot

Overshoot is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Settling

Settling is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. Noise

Noise is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Integral windup

Integral windup is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 11. Output limits

Output limits is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 12. Axis-specific control.

Axis-specific control. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Learn P, I and D separately before combining them. Understand what each term reacts to, what its gain changes, and why derivative action can amplify noisy measurements. Tune in simulation/bench environments before controlled flight.

# Labs

1) Simulate P control. 2) Add I. 3) Add D. 4) Observe overshoot and settling. 5) Test output saturation and anti-windup. 6) Tune a safe simulated/bench loop before flight testing.

# Gandaberunda connection

PID runs on the STM32 inside the stabilization loop. It receives desired and estimated state, computes corrections and passes them to the motor mixer.

# Must know

Error; P/I/D roles; gains; saturation; windup; noise; tuning; loop timing.

# Pass test

Both developers can explain P, I and D, predict the effect of changing each gain and explain why tuning must be performed progressively and safely.

# Deliverable

docs/learning/lab-notes/pid-tuning.md
