# Project Gandaberunda — V1 Software Architecture

> **Purpose:** Define how the Gandaberunda V1 firmware is organized and how the major software modules interact.

---

## 1. Software Philosophy

The firmware should be:

- Modular
- Understandable
- Testable
- Maintainable
- Reusable
- Easy to debug
- Expandable

We will **not** build the entire flight controller as one large program.

Each subsystem should have a clear responsibility.

---

## 2. High-Level Software Architecture

```text
                    ┌─────────────────────┐
                    │   Controller ESP32  │
                    │                     │
                    │ Input Processing    │
                    │ Packet Formation    │
                    │ OLED                │
                    └──────────┬──────────┘
                               │
                            ESP-NOW
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Drone ESP32      │
                    │                     │
                    │ Packet Reception    │
                    │ Validation          │
                    │ Communication       │
                    └──────────┬──────────┘
                               │
                              UART
                               │
                               ▼
              ┌─────────────────────────────────┐
              │              STM32               │
              │                                 │
              │  ┌───────────────────────────┐  │
              │  │ Communication             │  │
              │  └─────────────┬─────────────┘  │
              │                │                │
              │  ┌─────────────▼─────────────┐  │
              │  │ Flight State              │  │
              │  └─────────────┬─────────────┘  │
              │                │                │
              │  ┌─────────────▼─────────────┐  │
              │  │ Sensor Processing         │  │
              │  └─────────────┬─────────────┘  │
              │                │                │
              │  ┌─────────────▼─────────────┐  │
              │  │ Flight Controller / PID  │  │
              │  └─────────────┬─────────────┘  │
              │                │                │
              │  ┌─────────────▼─────────────┐  │
              │  │ Motor Mixer               │  │
              │  └─────────────┬─────────────┘  │
              └─────────────────┼────────────────┘
                                │
                                ▼
                             ESCs
                                │
                                ▼
                             Motors
```

---

## 3. Firmware Layers

The STM32 firmware will use a layered architecture.

```text
Application Layer
       ↓
Flight Control Layer
       ↓
State / Safety Layer
       ↓
Sensor & Communication Layer
       ↓
Hardware Driver Layer
       ↓
STM32 Hardware
```

Each layer should depend only on the layers below it where practical.

---

## 4. Application Layer

The application layer manages the overall system.

Responsibilities include:

- Startup
- System state
- Flight-mode selection
- Main scheduling
- System status
- Coordination between modules

It should not directly manipulate hardware registers.

---

## 5. Flight-Control Layer

This is the core of the drone.

Responsibilities include:

- Reading the desired pilot commands
- Reading estimated vehicle state
- Calculating control errors
- Running PID controllers
- Generating control outputs
- Passing outputs to the motor mixer

Basic flow:

```text
Pilot Command
      +
Vehicle State
      ↓
Control Error
      ↓
PID
      ↓
Control Output
```

---

## 6. State Estimation

The flight controller needs to determine the current state of the drone.

Sensor data is processed to estimate information such as:

- Roll
- Pitch
- Yaw
- Angular velocity
- Vertical information where available

Conceptually:

```text
Raw Sensors
    ↓
Filtering
    ↓
Sensor Processing
    ↓
State Estimation
    ↓
Flight Controller
```

The exact estimation method will be developed and tested during V1.

---

## 7. Sensor Layer

The sensor layer provides a consistent interface to sensors.

Possible sensors include:

- IMU
- ToF
- GPS
- Magnetometer
- Battery voltage measurement

Example:

```text
Flight Controller
       ↓
Sensor Interface
       ↓
Sensor Driver
       ↓
Physical Sensor
```

The flight-control code should not need to know the low-level details of an individual sensor.

---

## 8. Sensor Drivers

Each sensor should have its own driver module.

Example:

```text
drivers/
├── imu/
├── tof/
├── gps/
└── magnetometer/
```

A driver should handle:

- Initialization
- Configuration
- Data acquisition
- Basic validation
- Hardware communication

Higher-level processing should remain outside the driver.

---

## 9. Communication Layer

The communication layer handles commands received from the controller.

Flow:

```text
ESP-NOW
   ↓
ESP32 Receiver
   ↓
UART
   ↓
STM32 Communication Module
   ↓
Packet Validation
   ↓
Command Processing
```

Communication code should be separate from flight-control code.

---

## 10. Command Data

The controller sends structured commands rather than raw hardware instructions.

Conceptually:

```text
Command Packet

├── Packet Type
├── Sequence Number
├── Timestamp / Timing Information
├── Throttle
├── Roll
├── Pitch
├── Yaw
├── Flight Mode
├── ARM / DISARM
└── Integrity Information
```

The exact packet format will be defined in:

```text
docs/communication/communication-architecture.md
```

---

## 11. Packet Validation

Incoming packets must be validated before being used.

Validation should consider:

- Packet structure
- Packet type
- Data ranges
- Sequence information
- Timing
- Integrity information

Invalid packets must not directly affect motor commands.

---

## 12. Safety Layer

Safety logic has higher priority than normal flight commands.

It monitors conditions such as:

- Communication
- Sensor status
- Battery
- ARM state
- Flight state
- System faults

Conceptually:

```text
Normal Command
       │
       ▼
Safety Checks
       │
       ├── Valid → Continue
       │
       └── Invalid → Safe Response
```

---

## 13. ARM / DISARM Software

The software should maintain an explicit system state.

Example:

```text
BOOT
  ↓
INITIALIZING
  ↓
READY
  ↓
DISARMED
  ↓
ARMING CHECK
  ↓
ARMED
  ↓
FLIGHT
  ↓
DISARMED
```

The system must not automatically arm after startup or reset.

---

## 14. Flight Modes

The initial software architecture supports multiple flight modes.

Examples:

```text
MANUAL
STABILIZED
AUTONOMOUS
FAILSAFE
```

V1 will primarily focus on reliable manual/stabilized flight.

Additional modes will be introduced only after the basic flight-control system is reliable.

---

## 15. PID Controller

The PID controller is responsible for reducing the difference between desired and measured behaviour.

Conceptually:

```text
Desired State
      │
      ▼
    Error
      │
      ▼
     PID
      │
      ▼
Control Output
```

PID will be introduced progressively.

We will first understand:

1. Feedback
2. Error
3. Proportional control
4. Integral control
5. Derivative control
6. PID tuning
7. Practical flight behaviour

PID values will not be copied blindly from another drone.

---

## 16. Motor Mixer

The motor mixer converts flight-control outputs into individual motor commands.

For a quadrotor:

```text
Roll
Pitch
Yaw
Throttle
   │
   ▼
Motor Mixer
   │
   ├── Motor 1
   ├── Motor 2
   ├── Motor 3
   └── Motor 4
```

The exact motor numbering and mixing matrix will be documented separately.

---

## 17. ESC Output Layer

The STM32 generates ESC commands.

V1:

```text
STM32 → PWM → ESC
```

Future:

```text
STM32 → DShot → ESC
```

The ESC-output module should hide the protocol-specific details from the flight-control code.

---

## 18. Timing Architecture

Flight control depends heavily on timing.

The firmware must distinguish between:

- High-frequency control tasks
- Sensor updates
- Communication
- Safety monitoring
- Lower-priority tasks
- Display/debugging

The main control loop must have predictable timing.

Debug printing must not unnecessarily block the control loop.

---

## 19. Main Firmware Flow

A simplified firmware cycle is:

```text
Read Sensors
     ↓
Validate Sensor Data
     ↓
Update State Estimate
     ↓
Receive / Process Commands
     ↓
Check Safety
     ↓
Run Flight Controller
     ↓
Run Motor Mixer
     ↓
Output Motor Commands
     ↓
Update System Status
```

The exact scheduling mechanism will be finalized during implementation.

---

## 20. Error Handling

Software errors should be handled explicitly.

Examples:

```text
Sensor Failure
     ↓
Mark Sensor Invalid
     ↓
Notify Safety Layer
     ↓
Determine Safe Response
```

The system should avoid silently using invalid sensor data.

---

## 21. Debugging Strategy

Debugging will proceed from the lowest layer upward.

```text
Hardware
   ↓
Driver
   ↓
Sensor Data
   ↓
Communication
   ↓
State Estimation
   ↓
PID
   ↓
Motor Output
   ↓
Full Flight
```

We should not change multiple unrelated modules at the same time when diagnosing a failure.

---

## 22. Configuration

Values that are expected to change during development should not be scattered throughout the code.

Examples include:

- PID gains
- Sensor configuration
- Control limits
- Communication timeout
- Battery thresholds
- Motor limits

Configuration should be centralized where practical.

---

## 23. Logging and Diagnostics

The firmware should provide useful diagnostic information during development.

Possible information:

- Sensor status
- Communication status
- Battery voltage
- Flight mode
- ARM state
- Loop timing
- Sensor values
- Fault conditions

Debugging features must not interfere with the real-time flight-control loop.

---

## 24. Firmware Folder Structure

The exact structure may evolve, but the initial architecture should follow this concept:

```text
firmware/
├── stm32/
│   ├── app/
│   ├── control/
│   ├── communication/
│   ├── sensors/
│   ├── safety/
│   ├── drivers/
│   ├── motors/
│   ├── config/
│   └── main/
│
└── esp32/
    ├── controller/
    └── receiver/
```

The structure should remain modular as the project grows.

---

## 25. Controller Firmware

The controller firmware should be separated from the drone firmware.

Controller responsibilities:

```text
Input Drivers
      ↓
Input Processing
      ↓
Command Generation
      ↓
Packet Formation
      ↓
ESP-NOW
      ↓
OLED / Status
```

The controller should not contain flight-control algorithms.

---

## 26. Receiver Firmware

The drone ESP32 firmware should remain lightweight.

Its main purpose is:

```text
Receive
   ↓
Validate
   ↓
Forward
```

The ESP32 should not become a second flight controller.

---

## 27. Software Upgrade Path

The architecture allows future additions such as:

- Better state estimation
- Improved PID control
- Advanced flight modes
- Autonomous navigation
- Companion computer
- Computer vision
- Additional sensors
- Advanced communication

These should be added as modules rather than rewriting the entire firmware.

---

## 28. Development Order

Software development will follow:

```text
MCU Basics
   ↓
GPIO / Timers
   ↓
UART
   ↓
SPI / I2C
   ↓
Sensor Drivers
   ↓
ESP-NOW
   ↓
Command Protocol
   ↓
Motor / ESC Control
   ↓
State Estimation
   ↓
PID
   ↓
Safety
   ↓
Integration
   ↓
Ground Testing
   ↓
Flight Testing
```

---

## 29. Software Rules

The project follows these rules:

### Rule 1

Do not put the entire firmware in one file.

### Rule 2

Drivers should not contain flight-control logic.

### Rule 3

Communication should not directly control motors.

### Rule 4

Safety logic must be independent of normal commands.

### Rule 5

Configuration values should be centralized.

### Rule 6

Functions should have clear responsibilities.

### Rule 7

Avoid unnecessary blocking operations in the flight loop.

### Rule 8

Test modules individually before integration.

### Rule 9

Both developers must understand the complete firmware architecture.

### Rule 10

Do not add complexity without a clear engineering reason.

---

## 30. V1 Software Boundary

### Required for V1

- Controller firmware
- ESP-NOW communication
- UART communication
- STM32 firmware
- Sensor drivers
- IMU processing
- Flight modes
- PID control
- Motor mixing
- ESC output
- ARM / DISARM
- Failsafe
- Diagnostics

### Future

- DShot
- Advanced state estimation
- Autonomous navigation
- Computer vision
- Companion computer
- Additional sensing
- Advanced mission logic

---

## 31. Final Principle

> **The software should be simple enough for two engineers to understand completely, but modular enough to grow into a larger robotics platform.**

V1 is successful when the firmware is:

**Understandable → Testable → Reliable → Modular → Upgradeable**

---

**Status:** V1 Software Architecture — Initial Baseline