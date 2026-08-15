# 🦅 Project Gandaberunda

> **A low-cost, modular drone platform for remote-area sensing and future intelligent sensing research.**

## Developers
- Vishnu Kaushik S
- Vikas DK

Project Gandaberunda is a student-built modular drone platform being developed from the ground up with a focus on:

- Reliable flight
- Embedded systems
- Flight control
- Wireless communication
- Sensor integration
- Safety and failsafe systems
- Autonomous capabilities
- Modular hardware/software architecture
- Future sensing research

The goal is not simply to build a drone that flies.

The goal is to build a **reliable engineering platform that can continuously evolve through multiple versions.**

---

## 📌 Project Status

**Current Version:** V1 — Flight Platform  
**Development Stage:** Research / Architecture / Prototype Development  
**Team:** 2 student developers  
**Primary Controller:** STM32  
**Wireless System:** ESP32 + ESP-NOW  
**Repository Status:** Active Development

---

# 📖 Table of Contents

- [Project Vision](#-project-vision)
- [Version Roadmap](#-version-roadmap)
- [V1 Goals](#-v1---flight-platform)
- [System Architecture](#-system-architecture)
- [Communication Architecture](#-communication-architecture)
- [Safety Architecture](#-safety-architecture)
- [Development Timeline](#-development-timeline)
- [Budget](#-budget)
- [Repository Structure](#-repository-structure)
- [Git Workflow](#-git-workflow)
- [Engineering Approach](#-engineering-approach)
- [Testing Strategy](#-testing-strategy)
- [Future Research](#-future-research)
- [Team](#-team)
- [Project Philosophy](#-project-philosophy)

---

# 🚁 Project Vision

Gandaberunda is being developed as a **modular aerial sensing platform**.

The development philosophy is:

```text
Reliable Flight
      ↓
Modular Platform
      ↓
Sensing Platform
      ↓
Intelligent Sensing
      ↓
Autonomous Remote-Area System

---

# 🛠️ Version Roadmap

Gandaberunda will be developed through multiple versions.

Each version will build upon the previous one rather than completely replacing it.

## V0 — Research & Engineering Foundation

**Purpose:** Understand the fundamentals before building the aircraft.

### Main Topics

- Drone fundamentals
- Quad-X configuration
- Brushless motors
- ESCs
- Propellers
- Batteries
- Power systems
- IMU
- Accelerometer
- Gyroscope
- Magnetometer
- PID control
- Flight stabilization
- PWM
- DShot
- STM32
- ESP32
- ESP-NOW
- UART
- SPI
- Sensor calibration
- Failsafe concepts
- Embedded debugging

### Outcome

A clear understanding of the complete system architecture and component requirements before major hardware integration.

---

# V1 — Flight Platform

**Primary objective:** Build a reliable and repeatable flying platform.

### Core Features

- STM32-based flight controller
- Custom ESP32 controller
- ESP-NOW communication
- Quad-X configuration
- IMU-based stabilization
- Cascaded PID control
- PWM-based ESC control initially
- Battery monitoring
- 0.96" OLED diagnostics
- Manual ARM / DISARM
- Separate emergency kill
- Communication failsafe
- Sensor validation
- Watchdog and reset handling
- Downward ToF sensor
- Controlled autonomous landing
- Optional NEO-6M GPS
- Indoor-capable basic flight

### V1 Success Criteria

The system should be able to:

1. Fly safely.
2. Hover and stabilize predictably.
3. Accept commands from the custom controller.
4. Detect communication problems.
5. Enter an appropriate failsafe state.
6. Perform controlled autonomous landing.
7. Operate basic flight without depending on GPS.
8. Provide useful system information through the OLED.

### V1 Principle

> **Stable manual flight comes before autonomous features.**

---

# V2 — Advanced Sensing Platform

**Primary objective:** Turn the reliable V1 aircraft into a modular sensing platform.

### Planned Features

- Thermal sensing
- Night vision
- Additional environmental sensors
- Improved sensing payload interface
- Improved positioning
- Better telemetry
- Improved flight-state management
- Additional autonomous capabilities

V2 should add capabilities without requiring a complete redesign of the V1 platform.

---

# V3 — Intelligent Sensing Platform

**Primary objective:** Add higher-level processing and environmental understanding.

### Possible Directions

- Companion computer
- Computer vision
- Object detection
- Environmental classification
- Advanced sensor fusion
- Mapping
- Improved autonomous navigation
- Higher-level mission planning

V3 will be defined after V2 testing and evaluation.

---

# V4 — Autonomous Remote-Area Platform

**Primary objective:** Move from a remotely controlled sensing platform toward a more autonomous system.

### Possible Capabilities

- Autonomous navigation
- Waypoint missions
- Mapping
- Obstacle awareness
- Automated data collection
- Intelligent mission planning
- Multi-sensor environmental understanding

---

# V5+ — Research Platform

Future versions will be driven by research results rather than a fixed feature list.

### Potential Research Areas

- Advanced sensing
- Environmental sensing
- Indoor environment understanding
- Human presence detection
- Advanced mapping
- Autonomous navigation
- Multi-modal sensing
- Other emerging sensing technologies

> Future research directions are exploratory and will be validated experimentally before becoming part of the platform.

---

# 🧠 System Architecture

Current V1 architecture:

```text
                  CUSTOM CONTROLLER
                       ESP32 TX
                          │
                          │ ESP-NOW
                          ▼
                       ESP32 RX
                          │
                          │ UART
                          ▼
                 STM32 FLIGHT CONTROLLER
                          │
              ┌───────────┼───────────┐
              │           │           │
             IMU       CONTROL      SAFETY
              │           │           │
              └───────────┼───────────┘
                          │
                     MOTOR MIXER
                          │
              ┌───────────┼───────────┐
              ▼           ▼           ▼
             ESC         ESC         ESC       ESC
              │           │           │         │
             M1          M2          M3        M4
