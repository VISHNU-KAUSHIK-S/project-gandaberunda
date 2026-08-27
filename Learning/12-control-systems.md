# Project Gandaberunda — 12 Control Systems

Project Gandaberunda • Both developers complete every section

# Prerequisites

Flight dynamics and sensor estimation.

# What to learn

Setpoint, feedback, error, open-loop vs closed-loop control, stability, response, saturation, loop rate, actuator limits and motor mixing.

# Explanation

## 1. Setpoint

A setpoint is the desired value the controller is trying to achieve. For a stabilized axis, it can represent the desired attitude or related command.

## 2. Feedback

Feedback is the measured state returned to the controller. The IMU supplies measurements that let the STM32 compare what the drone is doing with what it should be doing.

## 3. Error

Error is the difference between the desired value and measured value: error = setpoint − measurement. The controller uses this error to determine the correction required.

## 4. Open-loop vs closed-loop control

Closed-loop control continuously measures the system and uses that measurement to correct the output. Gandaberunda's stabilization loop follows desired command → error → controller → motors → drone response → sensor feedback.

## 5. Stability

Stability means the response remains controlled rather than diverging. A disturbance should produce a correction that brings the aircraft back toward the desired state instead of causing growing oscillation.

## 6. Response

Response is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Saturation

Saturation occurs when the controller requests more actuator output than the motor/ESC system can provide. The output then reaches its physical limit, which must be considered during control design and tuning.

## 8. Loop rate

Loop rate is how frequently a control process repeats. A fixed loop rate gives the controller a predictable time base; timing jitter changes when calculations occur and can affect behaviour.

## 9. Actuator limits

Actuator limits is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 10. Motor mixing.

Motor mixing converts collective and axis corrections into individual motor commands. It is the bridge between control outputs such as roll/pitch/yaw and the four physical actuators.

# How to study

Start with a simple feedback example. Learn each term separately, then draw the complete flight-control loop. Study how measured attitude is compared with the desired attitude and how controller output reaches the motors.

# Labs

1) Simulate a simple feedback loop. 2) Apply a step input. 3) Observe overshoot/settling. 4) Add output limits. 5) Trace controller output through a safe motor-mixer test.

# Gandaberunda connection

Gandaberunda uses sensor feedback on the STM32 to turn desired pilot commands into stabilized motor outputs.

# Must know

Setpoint, feedback, error, closed-loop control, stability, saturation, loop rate and motor mixing.

# Pass test

Both developers can draw and explain the full feedback loop and explain how a measured error becomes individual motor commands.

# Deliverable

docs/learning/lab-notes/control-loop-notes.md
