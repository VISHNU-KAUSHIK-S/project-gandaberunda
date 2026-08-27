# Project Gandaberunda — Learning Program

Project Gandaberunda • Both developers complete every section

This learning program is written to teach the engineering concepts required for Gandaberunda V1. Each module follows the same format:

- Prerequisites
- What to learn
- Explanation of every listed concept
- How to study
- Labs
- Gandaberunda connection
- Must know
- Pass test
- Deliverable

## V1 architecture

```text
CUSTOM CONTROLLER
      │
    ESP32
      │
   ESP-NOW
      │
      ▼
ESP32 RECEIVER
      │
     UART
      │
      ▼
STM32 FLIGHT CONTROLLER
      │
      ├── Sensors
      ├── State Estimation
      ├── Flight Modes
      ├── PID Control
      ├── Motor Mixing
      └── Safety / Failsafe
              │
             PWM
              │
              ▼
             ESCs
              │
              ▼
            Motors
```

The STM32 is the final authority over motor operation.

## Learning modules

01 Electronics  
02 Embedded Systems  
03 STM32  
04 Flight Dynamics  
05 BLDC Motors  
06 ESC & Motor Control  
07 LiPo Batteries  
08 Propulsion & Thrust  
09 Power Systems  
10 IMU & Sensors  
11 Sensor Fusion  
12 Control Systems  
13 PID Control  
14 PWM  
15 UART  
16 SPI  
17 ESP-NOW  
18 Communication Protocol Design  
19 Safety & Failsafe  
20 Software Engineering  
21 Testing & Validation  
22 Debugging  
23 Procurement Readiness  
24 Day-by-Day Roadmap

## Learning rule

Both developers must understand the complete system. Implementation can be divided; system knowledge cannot.

## Safety

Use propeller-off bench testing during early motor/ESC work. Fault injection must use safe low-energy or simulated conditions. Do not intentionally create dangerous faults in energized propulsion hardware.

## Evidence

Use `docs/learning/lab-notes/` for calculations, diagrams, firmware evidence, measurements, test results, logs, debugging records and lessons learned.

> **Understand the system before relying on it. Build the simplest reliable V1 first. Test progressively. Document failures. Then upgrade.**
