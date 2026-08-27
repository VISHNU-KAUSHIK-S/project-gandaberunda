# Project Gandaberunda — 11 Sensor Fusion

Project Gandaberunda • Both developers complete every section

# Prerequisites

IMU/sensor fundamentals and basic control concepts.

# What to learn

State estimation, complementary filtering, low-pass filtering, accelerometer/gyro characteristics, drift, noise, coordinate frames and the idea of combining measurements.

# Explanation

## 1. State estimation

State estimation is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Complementary filtering

Complementary filtering is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Low-pass filtering

Low-pass filtering is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Accelerometer/gyro characteristics

Accelerometer/gyro characteristics is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Drift

Drift is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Noise

Noise is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Coordinate frames

Coordinate frames is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. The idea of combining measurements.

The idea of combining measurements. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Understand why no single sensor gives a perfect attitude estimate. Learn that gyroscopes respond well to short-term motion but drift, while accelerometers provide a gravity reference but are disturbed by acceleration. Study how filtering/fusion combines useful information.

# Labs

1) Record stationary gyro data. 2) Integrate gyro data conceptually and observe drift. 3) Observe accelerometer gravity direction. 4) Compare filtered signals. 5) Implement or simulate a simple complementary filter.

# Gandaberunda connection

The STM32 needs a usable estimate of attitude/state before the PID controller can make meaningful corrections.

# Must know

Drift; noise; gravity reference; filtering; complementary filter concept; coordinate frames; estimator timing.

# Pass test

Both developers can explain why sensor fusion is needed and can describe the information contributed by the gyro and accelerometer in a basic attitude estimate.

# Deliverable

docs/learning/lab-notes/sensor-fusion.md
