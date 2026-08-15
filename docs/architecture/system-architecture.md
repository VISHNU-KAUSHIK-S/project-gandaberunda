# Project Gandaberunda — V1 System Architecture

> **Purpose:** Define the overall structure of Gandaberunda V1 and how its major subsystems communicate.

---

## 1. V1 Goal

Gandaberunda V1 is a modular drone platform designed to achieve:

- Stable manual flight
- Stable hovering
- Custom controller operation
- Reliable communication
- Basic safety and failsafe behaviour
- A hardware/software architecture that can be upgraded later

**Priority:**

> Safety → Reliability → Understanding → Flight → Features → Upgrades

Advanced features will only be added after basic flight is reliable.

---

## 2. High-Level Architecture

```text
                    CUSTOM CONTROLLER
                   ┌──────────────────┐
                   │      ESP32       │
                   │                  │
                   │ Joysticks        │
                   │ Switches         │
                   │ ARM / DISARM     │
                   │ OLED             │
                   └────────┬─────────┘
                            │
                         ESP-NOW
                            │
                            ▼
                   ┌──────────────────┐
                   │   ESP32 RECEIVER │
                   │                  │
                   │ Packet Validation│
                   │ Communication    │
                   └────────┬─────────┘
                            │
                         UART
                     (SPI later)
                            │
                            ▼
                 ┌──────────────────────┐
                 │   STM32 FLIGHT       │
                 │     CONTROLLER       │
                 │                      │
                 │ Sensors              │
                 │ State Estimation     │
                 │ Flight Modes         │
                 │ PID Control          │
                 │ Motor Mixing         │
                 │ Safety / Failsafe    │
                 └──────────┬───────────┘
                            │
                         PWM V1
                      DShot later
                            │
                            ▼
                    ┌──────────────┐
                    │     ESCs     │
                    └──────┬───────┘
                           │
                           ▼
                       MOTORS
```

---

## 3. Main Subsystems

Gandaberunda V1 is divided into five main subsystems:

### 3.1 Controller

The controller is responsible for:

- Reading pilot inputs
- Sending commands
- Selecting flight modes
- ARM / DISARM
- Emergency control
- Displaying basic system information

The controller uses an ESP32.

---

### 3.2 Communication

The communication system connects the controller to the drone.

```text
Controller ESP32
       │
       │ ESP-NOW
       ▼
Drone ESP32
       │
       │ UART
       ▼
STM32
```

V1 uses:

**ESP32 → ESP-NOW → ESP32 → UART → STM32**

SPI may be evaluated later if testing shows a meaningful improvement.

The communication system is responsible for transporting commands.

It is **not responsible for flight control**.

---

### 3.3 Flight Controller

The STM32 is the core flight-control computer.

It is responsible for:

- Reading sensors
- Sensor calibration
- Sensor validation
- State estimation
- Flight modes
- Stabilization
- PID control
- Motor mixing
- Motor commands
- Arming/disarming
- Failsafe behaviour

The STM32 is the final authority over motor operation.

---

### 3.4 Sensors

The flight controller receives information from sensors such as:

- IMU
- ToF sensor
- GPS
- Optional magnetometer

The IMU is the primary sensor for stabilization.

Other sensors are added only when their information is required.

Basic stabilization must not depend on optional sensors.

---

### 3.5 Propulsion

The propulsion system consists of:

```text
STM32
   │
   ▼
 ESCs
   │
   ▼
Motors
   │
   ▼
Propellers
```

V1 initially uses PWM-based ESC control.

DShot may be introduced later.

---

## 4. Flight-Control Flow

The basic flight-control pipeline is:

```text
Sensors
   │
   ▼
Sensor Processing
   │
   ▼
State Estimation
   │
   ▼
Flight Mode
   │
   ▼
PID Controller
   │
   ▼
Motor Mixer
   │
   ▼
ESC Output
   │
   ▼
Motors
```

The controller provides desired pilot commands.

The STM32 converts those commands into safe motor outputs.

---

## 5. Control Authority

The architecture follows one important rule:

> **The STM32 always has final control authority over the motors.**

The ESP32 receiver cannot directly control the motors.

```text
Controller
    │
    ▼
Communication
    │
    ▼
STM32
    │
    ├── Accept command
    ├── Reject invalid command
    ├── Apply failsafe
    └── Generate motor output
             │
             ▼
           Motors
```

This keeps communication and flight control separate.

---

## 6. Flight Modes

V1 will initially support:

### Manual

The pilot provides the desired flight commands through the custom controller.

```text
Pilot
  │
  ▼
Controller
  │
  ▼
Communication
  │
  ▼
STM32
  │
  ▼
Flight Control
```

### Autonomous

The STM32 uses predefined control logic to generate desired behaviour.

```text
Autonomous Logic
       │
       ▼
Flight Controller
       │
       ▼
PID
       │
       ▼
Motors
```

Autonomous functionality will be introduced only after reliable manual flight.

---

## 7. Arming

The drone must never automatically arm after:

- Power-up
- Reset
- Communication recovery
- Flight-controller reboot

Normal operation follows:

```text
Power On
   │
   ▼
System Check
   │
   ▼
DISARMED
   │
   ▼
Operator ARM
   │
   ▼
ARMED
```

The system must verify required conditions before allowing motor operation.

---

## 8. Communication Safety

Every received command should be checked before being used.

Important checks include:

- Packet validity
- Packet timing
- Sequence information
- Command range
- Communication state

If valid communication is lost, the STM32 enters the appropriate failsafe behaviour.

The exact timeout values will be determined during testing.

---

## 9. Failsafe

The STM32 continuously monitors important system conditions.

```text
Communication
Sensor Status
Battery
Flight State
System Status
      │
      ▼
Safety / Failsafe Logic
      │
      ▼
Appropriate Response
```

Possible responses include:

- Continue flight
- Stabilize
- Hold temporarily
- Controlled landing
- Disarm

The exact response depends on the failure and current flight state.

---

## 10. Startup

Normal startup follows:

```text
POWER ON
   │
   ▼
Initialize Hardware
   │
   ▼
Initialize Sensors
   │
   ▼
Calibration / Validation
   │
   ▼
System Checks
   │
   ▼
DISARMED / READY
```

The system should clearly communicate its current state to the operator.

---

## 11. Reset During Flight

A flight-controller reset is treated differently from normal startup.

After recovery, the system should:

1. Reinitialize required hardware
2. Re-establish valid communication
3. Validate incoming commands
4. Restore safe control behaviour
5. Prevent unintended re-arming

The exact recovery strategy will be developed and tested separately.

---

## 12. Hardware Upgrade Boundaries

The architecture is designed so that major subsystems can be upgraded independently.

Examples:

```text
UART
  ↓
SPI

PWM
  ↓
DShot

Basic Sensors
  ↓
Additional Sensors

Basic Flight Control
  ↓
Advanced Autonomous Control
```

Future payloads such as:

- Night vision
- Thermal sensing
- Additional sensing systems
- Companion computing

should be integrated without requiring a complete redesign of V1.

---

## 13. Software Architecture

The firmware should remain modular.

Conceptually:

```text
Application
    │
    ▼
Flight Modes
    │
    ▼
Flight Control
    │
    ▼
Control Algorithms
    │
    ▼
Hardware Interfaces
    │
    ▼
Drivers
    │
    ▼
Hardware
```

Communication, sensors, control algorithms, safety logic, and hardware drivers should remain separate modules.

---

## 14. Development Strategy

Development will proceed in this order:

```text
Learn
  ↓
Requirements
  ↓
Architecture
  ↓
Hardware Selection
  ↓
Individual Hardware Testing
  ↓
Sensor Bring-up
  ↓
Communication Testing
  ↓
Motor / ESC Testing
  ↓
Flight Controller
  ↓
Calibration
  ↓
Stabilization
  ↓
Basic Flight
  ↓
Stable Hover
  ↓
Safety Validation
  ↓
Additional Features
```

Features will not be added simply for the sake of adding features.

---

## 15. Team Knowledge Principle

Both developers must understand the complete system.

Responsibilities may be divided during implementation, but knowledge must not be permanently divided.

Both developers should understand:

- Hardware
- Firmware
- Communication
- Flight control
- Sensors
- Safety
- Testing
- System architecture

This allows either developer to maintain and upgrade the platform independently.

---

## 16. Documentation Structure

Detailed information is maintained separately:

```text
docs/
├── architecture/
│   └── system-architecture.md
│
├── hardware/
│   └── hardware-overview.md
│
├── software/
│   └── software-architecture.md
│
├── communication/
│   └── communication-architecture.md
│
├── safety/
│   └── safety-architecture.md
│
├── testing/
│   └── test-strategy.md
│
├── decisions/
│   └── engineering-decisions.md
│
└── versions/
    └── roadmap.md
```

Each document should contain only the level of detail appropriate to its purpose.

---

## 17. V1 Boundary

### V1 must achieve

- Reliable manual control
- Stable flight
- Stable hover
- Custom controller
- Reliable communication
- Sensor calibration
- Basic failsafe
- ARM / DISARM
- Emergency control
- Basic diagnostics
- Modular architecture

### Later versions may add

- Improved communication
- DShot
- SPI
- Advanced autonomous flight
- Night vision
- Thermal sensing
- Computer vision
- Additional sensing payloads
- Advanced navigation

---

## 18. Architecture Principles

Gandaberunda follows these principles:

### Keep it simple

Do not introduce complexity without a clear benefit.

### Safety first

Safety decisions take priority over normal commands.

### Test progressively

Do not integrate everything at once.

### Understand before implementing

Both developers should understand each subsystem before relying on it.

### Modular by design

Hardware and software should be replaceable where practical.

### Build V1 first

Advanced features come after reliable basic flight.

### Design for upgrades

V1 should provide a foundation for future versions.

---

## 19. Current Architecture

**Status:** V1 Architecture Defined

Current communication architecture:

```text
ESP32
  │
  │ ESP-NOW
  ▼
ESP32
  │
  │ UART
  ▼
STM32
```

Current flight-control architecture:

```text
Sensors
   ↓
STM32
   ↓
PID
   ↓
Motor Mixer
   ↓
PWM
   ↓
ESCs
   ↓
Motors
```

Future interfaces such as SPI and DShot will be evaluated experimentally rather than assumed to be better.

---

## 20. Final Principle

> **Build the simplest reliable system first. Understand it completely. Test it. Then upgrade it.**

This is the foundation of Project Gandaberunda.