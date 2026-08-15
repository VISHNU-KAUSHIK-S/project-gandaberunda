# Project Gandaberunda — V1 Hardware Overview

> **Purpose:** Define the major hardware used by Gandaberunda V1, its role, interfaces, and upgrade path.

---

## 1. Hardware Philosophy

Gandaberunda is being designed as a **modular platform**, not a one-time drone.

Hardware selection should prioritize:

1. Safety
2. Reliability
3. Learning value
4. Availability
5. Cost
6. Weight
7. Upgradeability

We should avoid spending money on features that are not required for reliable V1 flight.

The V1 hardware should also leave enough room in the budget for future upgrades.

---

## 2. Main Hardware Architecture

```text
                     CONTROLLER

              ┌─────────────────────┐
              │       ESP32         │
              │                     │
              │  Joysticks          │
              │  Switches           │
              │  ARM / DISARM       │
              │  OLED               │
              └──────────┬──────────┘
                         │
                      ESP-NOW
                         │
                         ▼
              ┌─────────────────────┐
              │     ESP32 RX        │
              │                     │
              │ Communication       │
              │ Packet Validation   │
              └──────────┬──────────┘
                         │
                       UART
                    SPI - Future
                         │
                         ▼
              ┌─────────────────────┐
              │       STM32         │
              │  FLIGHT CONTROLLER  │
              │                     │
              │ Sensors             │
              │ State Estimation    │
              │ PID                 │
              │ Flight Modes        │
              │ Safety              │
              │ Motor Mixing        │
              └──────────┬──────────┘
                         │
                    PWM - V1
                  DShot - Future
                         │
                         ▼
              ┌─────────────────────┐
              │        ESCs         │
              └──────────┬──────────┘
                         │
                         ▼
                     MOTORS
                         │
                         ▼
                    PROPELLERS
```

---

## 3. Flight Controller — STM32

The STM32 is the primary flight-control processor.

It handles:

- IMU data
- Sensor processing
- State estimation
- PID control
- Flight modes
- Motor mixing
- ESC outputs
- ARM / DISARM
- Failsafe logic
- Battery monitoring

### Important rule

> The STM32 is the final authority over motor operation.

The ESP32 communication system must never directly control the motors.

---

## 4. Communication — ESP32

Two ESP32 devices are used initially.

### Controller ESP32

Located on the custom controller.

Responsible for:

- Reading joysticks
- Reading switches
- ARM / DISARM input
- Mode selection
- Emergency control
- Sending control packets
- Updating the OLED

### Drone ESP32

Located on the drone.

Responsible for:

- Receiving ESP-NOW packets
- Checking packet validity
- Checking packet timing
- Checking sequence information
- Passing valid commands to the STM32

---

## 5. Communication Interfaces

### V1

```text
ESP32 Controller
       │
    ESP-NOW
       │
       ▼
ESP32 Receiver
       │
      UART
       │
       ▼
STM32
```

### Future

SPI may be tested between the ESP32 and STM32.

The purpose of the experiment will be to compare:

- Latency
- Reliability
- CPU usage
- Complexity
- Debugging difficulty

SPI will only replace UART if testing provides a meaningful advantage.

---

## 6. IMU

The IMU is the primary flight sensor.

It provides:

- Accelerometer data
- Gyroscope data

It is used for:

- Attitude estimation
- Stabilization
- PID feedback

The IMU must be:

- Rigidly mounted
- Properly oriented
- Protected from excessive vibration
- Calibrated before flight

The exact IMU will be selected based on:

- Performance
- Availability
- Cost
- Documentation
- Software support

---

## 7. ToF Sensor

A downward-facing Time-of-Flight sensor may be used for:

- Ground-distance measurement
- Low-altitude awareness
- Landing assistance
- Landing verification

It is **not required for the fundamental stabilization loop**.

The flight controller must validate ToF readings before using them for automatic decisions.

---

## 8. GPS

GPS is an optional subsystem for V1.

Potential uses include:

- Position information
- Future return-to-home functionality
- Future autonomous navigation

GPS is **not required** for:

- Basic stabilization
- Manual flight
- Basic hovering

The drone should remain capable of basic flight without GPS.

---

## 9. Magnetometer

A magnetometer may be used for heading-related functions.

However, it can be affected by:

- Motors
- ESCs
- Battery
- High-current wiring

Therefore it is not considered essential for basic V1 stabilization.

If used, its placement and calibration will be tested carefully.

---

## 10. Motors

Gandaberunda will use four brushless motors for the V1 quadrotor configuration.

Motor selection depends on the complete propulsion system.

Important parameters include:

- Required thrust
- Motor KV
- Voltage
- Current
- Propeller compatibility
- Weight
- Efficiency
- Cost
- Availability

Motor selection must be done together with the:

```text
Motor
  +
Propeller
  +
ESC
  +
Battery
```

rather than selecting each component independently.

---

## 11. Propellers

Propellers directly affect:

- Thrust
- Current
- Efficiency
- Motor loading
- Vibration
- Flight behaviour

The selected propellers must match the chosen motors and battery.

Both rotational directions required by the quadrotor configuration must be available.

Propeller orientation must always be checked before flight.

---

## 12. ESCs

ESCs control the brushless motors.

V1 will initially use:

```text
STM32 → PWM → ESC → Motor
```

DShot may be introduced later.

ESC selection must consider:

- Motor current
- Battery voltage
- Current margin
- Weight
- Cooling
- Reliability
- PWM support
- Future DShot support
- Cost

---

## 13. Battery

The battery will be selected after the propulsion system is determined.

Important parameters include:

- Cell count / voltage
- Capacity
- Discharge capability
- Weight
- Physical size
- Connector
- Cost

The battery must provide sufficient current for the complete propulsion system while maintaining reasonable flight time.

Battery selection will therefore be based on measured or expected propulsion requirements rather than capacity alone.

---

## 14. Power System

The propulsion system and electronics require different power requirements.

Conceptually:

```text
                 BATTERY
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
       ESCs             Voltage Regulator
          │                   │
          ▼                   ▼
       Motors         STM32 / ESP32 / Sensors
```

The power system must consider:

- Voltage
- Current
- Voltage regulation
- Wire size
- Connectors
- Heat
- Electrical noise
- Grounding

Sensitive electronics should be protected from propulsion-related electrical noise as much as practical.

---

## 15. Battery Monitoring

The STM32 will monitor battery voltage.

Possible states:

```text
NORMAL
   │
   ▼
WARNING
   │
   ▼
CRITICAL
```

The exact thresholds will be determined experimentally.

Battery monitoring will be integrated into the safety system.

---

## 16. Controller Hardware

The custom controller will contain:

- ESP32
- Joysticks / control inputs
- ARM / DISARM control
- Flight-mode switch
- Emergency control
- OLED
- Required power supply

The controller should be designed so that individual controls can be replaced without redesigning the communication system.

---

## 17. OLED

The OLED provides basic operator feedback.

Possible information:

```text
LINK : OK
MODE : MANUAL
ARM  : OFF
BAT  : OK
IMU  : OK
GPS  : OK
SYS  : READY
```

The OLED is primarily a diagnostic and status interface.

---

## 18. Safety Hardware

The V1 system includes:

- ARM / DISARM control
- Emergency-kill control
- Main power control
- Proper battery connector
- Status indication

Safety controls should be physically and logically separated from normal flight commands where practical.

---

## 19. Mechanical Structure

The airframe must provide space and mounting for:

- Four motors
- ESCs
- Battery
- STM32
- ESP32 receiver
- Sensors
- Wiring
- Future payloads

The structure should not make future upgrades unnecessarily difficult.

We should avoid selecting a frame that is adequate only for the first version.

---

## 20. Sensor Placement

### IMU

Should have:

- Rigid mounting
- Low vibration
- Known orientation

### ToF

Should have:

- Clear downward view
- Stable mounting
- Minimal obstruction

### GPS

Should have:

- Clear sky visibility
- Suitable separation from interference

### Magnetometer

Should have:

- Maximum practical separation from high-current and magnetic interference sources

---

## 21. Wiring

Wiring should be:

- Secure
- Insulated
- Protected
- Clearly organized
- Away from propellers
- Away from moving parts

High-current propulsion wiring should be separated from sensitive signal wiring where practical.

Connectors should be mechanically secured.

---

## 22. Hardware Interfaces

Main interfaces:

```text
ESP32 TX → ESP-NOW → ESP32 RX

ESP32 RX → UART → STM32

IMU → STM32

ToF → STM32

GPS → STM32

STM32 → PWM → ESCs

ESCs → Motors
```

Future interfaces:

```text
ESP32 RX → SPI → STM32

STM32 → DShot → ESCs

STM32 → Companion Computer

Additional Sensors → STM32 / Companion Computer
```

---

## 23. Hardware Selection Rules

Every major component should be evaluated using:

| Parameter | Priority |
|---|---|
| Safety | Very High |
| Reliability | Very High |
| Cost | High |
| Availability | High |
| Weight | High |
| Performance | High |
| Documentation | High |
| Ease of integration | High |
| Future compatibility | High |
| Learning value | High |

The cheapest component is not automatically the best component.

The objective is:

> **Maximum engineering value per rupee.**

---

## 24. Budget Philosophy

The project has a student budget.

Therefore:

- Avoid unnecessary purchases
- Prefer reusable components
- Avoid buying advanced hardware before it is needed
- Prefer widely available components
- Keep spare budget for failures and future upgrades
- Reuse V1 hardware wherever possible
- Avoid designing V1 around hardware that cannot support future expansion

The goal is not to maximize the V1 bill.

The goal is to build a reliable platform while preserving budget for future versions.

---

## 25. Hardware Development Order

Hardware development will follow:

```text
Requirements
    ↓
Propulsion calculations
    ↓
Component research
    ↓
Component selection
    ↓
Individual testing
    ↓
Power testing
    ↓
Sensor testing
    ↓
Communication testing
    ↓
ESC / Motor testing
    ↓
System integration
    ↓
Ground testing
    ↓
Flight testing
```

No component should be integrated into flight testing before its basic operation has been verified.

---

## 26. Hardware Testing

Testing will progress from simple to complex:

```text
Power
  ↓
Communication
  ↓
Sensor Data
  ↓
Motor Output
  ↓
Subsystem Integration
  ↓
Full System
  ↓
Controlled Flight
```

If something fails, the likely failure source should be isolated before replacing components.

---

## 27. Hardware Status

### Confirmed Architecture

- STM32 flight controller
- ESP32 controller
- ESP32 drone receiver
- ESP-NOW communication
- UART between ESP32 and STM32
- Four-motor quadrotor architecture
- PWM ESC control for V1
- IMU-based stabilization

### Planned / To Be Finalized

- Exact STM32 board
- Exact ESP32 board
- Motors
- ESCs
- Propellers
- Battery
- IMU
- ToF sensor
- GPS
- Magnetometer
- Voltage regulator
- Exact mechanical structure

These components will be finalized after proper engineering evaluation.

---

## 28. Upgrade Path

The hardware should support future upgrades such as:

```text
V1
 │
 ├── SPI communication
 ├── DShot
 ├── Improved sensors
 ├── GPS-based functions
 │
 ▼
V2
 │
 ├── Night vision
 ├── Thermal sensing
 ├── Additional payloads
 │
 ▼
Future Research
 │
 ├── Companion computing
 ├── Computer vision
 ├── Advanced sensing
 └── Autonomous navigation
```

Future upgrades should reuse as much of the V1 platform as practical.

---

## 29. Final Principle

> **Do not buy hardware simply because it is available. Select hardware because it solves a defined requirement.**

V1 should be:

**Simple → Reliable → Understandable → Modular → Upgradeable**

The objective is to build a strong platform, not simply a drone that flies once.

---

**Status:** V1 Hardware Architecture — Initial Baseline