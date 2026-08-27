# Project Gandaberunda — 14 PWM

Project Gandaberunda • Both developers complete every section

# Prerequisites

Embedded systems, timers and ESC concepts.

# What to learn

Duty cycle, frequency, timer configuration, pulse timing, PWM generation, measurement, command mapping and ESC signal requirements.

# Explanation

## 1. Duty cycle

Duty cycle is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 2. Frequency

Frequency is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 3. Timer configuration

Timer configuration is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 4. Pulse timing

Pulse timing is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 5. Pwm generation

PWM is a timed digital waveform whose timing characteristics encode a command. In V1 the STM32 uses PWM to command ESCs.

## 6. Measurement

Measurement is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 7. Command mapping

Command mapping is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

## 8. Esc signal requirements.

Esc signal requirements. is a concept that must be understood in the context of the drone subsystem that uses it. Learn its definition, inputs and outputs, normal behaviour, limits, and failure modes before relying on it in Gandaberunda.

# How to study

Learn PWM as a timed digital waveform rather than simply a percentage. Configure a timer, generate a waveform and measure it. Then map the correct command range to the V1 ESC interface.

# Labs

1) Generate PWM from STM32. 2) Measure period and pulse timing. 3) Change duty/timing. 4) Verify output safely. 5) Test the ESC command range without propellers.

# Gandaberunda connection

V1 uses STM32 PWM to command the ESCs. DShot is a later interface boundary.

# Must know

Frequency/period; duty cycle; timer compare; pulse timing; command range; measurement.

# Pass test

Both developers can configure and measure PWM and explain exactly how the STM32's timer output becomes an ESC command.

# Deliverable

docs/learning/lab-notes/pwm.md
