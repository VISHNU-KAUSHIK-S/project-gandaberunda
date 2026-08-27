# Project Gandaberunda — 10 IMU & Sensors

Project Gandaberunda • Both developers complete every section

# Prerequisites

Embedded systems, STM32 and flight dynamics.

# What to learn

Accelerometer, gyroscope, IMU axes, coordinate frames, units, bias, scale error, noise, calibration, sampling, filtering, ToF, GPS and optional magnetometer roles.

# Explanation

## 1. Accelerometer

Accelerometer is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Gyroscope

Gyroscope is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Imu axes

An IMU combines motion sensors such as an accelerometer and gyroscope. The gyro measures angular rate while the accelerometer measures specific force; together they provide core information for stabilization.

## 4. Coordinate frames

Coordinate frames is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Units

Units is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 6. Bias

Bias is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Scale error

Error is the difference between the desired value and measured value: error = setpoint − measurement. The controller uses this error to determine the correction required.

## 8. Noise

Noise is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 9. Calibration

Calibration is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Sampling

Sampling is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 11. Filtering

Filtering is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 12. Tof

Tof is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 13. Gps

Gps is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 14. Optional magnetometer roles.

Optional magnetometer roles. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Learn what each sensor actually measures before using it. Then study axes, units, calibration and error sources. Finally learn how raw measurements become information useful to stabilization and navigation.

# Labs

1) Read raw IMU data. 2) Observe stationary noise/bias. 3) Calibrate. 4) Rotate sensor and map axes. 5) Compare filtered/unfiltered data. 6) Test invalid sensor handling.

# Gandaberunda connection

The IMU is the primary stabilization sensor. ToF, GPS and optional magnetometer provide additional information only where required; basic stabilization must not depend on optional sensors.

# Must know

Accelerometer vs gyroscope; bias; noise; axes; calibration; sampling; filtering; sensor validity.

# Pass test

Both developers can explain each primary sensor, calibrate the IMU, identify invalid data and explain why sensor axes must match the flight-control coordinate system.

# Deliverable

docs/learning/lab-notes/imu-calibration.md and sensor-test notes.
