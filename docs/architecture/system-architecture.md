# Project Gandaberunda — V1 System Architecture

> **Purpose:** Define the overall architecture of the Gandaberunda V1 drone before detailed hardware and software implementation.

---

# 1. Document Purpose

This document defines the high-level architecture of Gandaberunda V1.

It describes:

- Major subsystems
- Responsibilities of each subsystem
- Communication paths
- Control flow
- Safety architecture
- Flight modes
- Upgrade boundaries
- System-level design decisions

This document acts as the high-level reference for V1 development.

Detailed implementation information will be maintained separately in the `docs/` directory.

---

# 2. V1 Objective

The primary objective of V1 is to build a reliable and understandable drone platform capable of:

- Stable manual flight
- Basic hovering
- Custom controller input
- Sensor-based stabilization
- Communication-loss handling
- Safe arming and disarming
- Emergency shutdown
- Controlled autonomous behaviour
- Basic system diagnostics
- Future expansion without major redesign

V1 prioritizes:

1. Safety
2. Reliability
3. Understanding
4. Stability
5. Debuggability
6. Modularity
7. Upgradeability
8. Cost efficiency

Advanced features will only be introduced after reliable basic flight has been achieved.

---

# 3. High-Level System

The V1 system consists of five major layers:

```text
+------------------------------------------------------+
|                  HUMAN / CONTROLLER                  |
|                                                      |
|        Custom Controller + Controls + OLED           |
+---------------------------+--------------------------+
                            |
                            | ESP-NOW
                            v
+------------------------------------------------------+
|                    COMMUNICATION                     |
|                                                      |
|                  ESP32 Receiver                     |
+---------------------------+--------------------------+
                            |
                            | UART
                            | (SPI may be evaluated later)
                            v
+------------------------------------------------------+
|                  FLIGHT CONTROLLER                   |
|                                                      |
|                      STM32                           |
|                                                      |
|  Sensor Processing → State Estimation → Control     |
|                    → Motor Mixing                    |
+---------------------------+--------------------------+
                            |
                            | PWM initially
                            | DShot future upgrade
                            v
+------------------------------------------------------+
|                    ACTUATION                         |
|                                                      |
|             ESCs → Brushless Motors                 |
+------------------------------------------------------+

                    SENSOR INPUTS
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
         IMU            ToF          Optional GPS
```

The architecture is designed so that communication, flight control, sensing, and actuation remain separate subsystems.

---

# 4. Main Components

## 4.1 Flight Controller — STM32

The STM32 is the primary flight-control processor.

Its responsibilities include:

- Reading the IMU
- Reading other flight sensors
- Sensor calibration
- Sensor validation
- State estimation
- Flight-mode management
- Stabilization
- PID control
- Motor mixing
- ESC command generation
- Arming/disarming logic
- Failsafe handling
- Battery monitoring
- Landing control
- System diagnostics
- Watchdog handling

The STM32 is the final authority for flight safety.

The communication processor must not directly control the motors.

---

## 4.2 Controller — ESP32

The custom controller will initially use an ESP32 as the transmitter processor.

Its responsibilities include:

- Reading control inputs
- Reading mode switches
- Reading ARM / DISARM input
- Reading emergency-kill input
- Creating control packets
- Transmitting packets using ESP-NOW
- Displaying system information on the OLED
- Providing operator feedback

The controller does not directly control the motors.

---

## 4.3 Receiver — ESP32

A second ESP32 will act as the receiver on the drone.

Its responsibilities include:

- Receiving ESP-NOW packets
- Validating received packets
- Checking packet timing
- Checking packet sequence information
- Forwarding valid commands to the STM32

The receiver acts as a communication bridge between the controller and flight controller.

---

## 4.4 ESCs and Motors

The STM32 generates motor-control commands.

These commands are sent to the ESCs.

The ESCs control the brushless motors.

```text
STM32
  |
  | Motor command
  v
ESC
  |
  | Electrical power switching
  v
Brushless Motor
```

V1 will initially use PWM-based ESC control.

DShot may be introduced as a later upgrade.

---

# 5. Controller Architecture

The custom controller contains:

- ESP32 transmitter
- Control inputs
- ARM / DISARM switch
- Manual / Autonomous mode switch
- Emergency-kill control
- OLED display
- Other required operator controls

The communication path is:

```text
Custom Controller
       |
       | ESP-NOW
       v
ESP32 Receiver
       |
       | UART
       v
STM32 Flight Controller
```

The controller provides commands and operator feedback, while the STM32 remains responsible for deciding whether those commands are safe to execute.

---

# 6. Communication Architecture

The initial communication path is:

```text
Controller
    |
    | ESP-NOW
    v
ESP32 RX
    |
    | UART
    v
STM32 Flight Controller
```

The communication system is designed for low-latency control input.

Packets will be validated before being accepted by the flight controller.

Validation will include:

- Packet identity
- Packet timing
- Sequence information
- Data integrity checking
- Command range validation

The exact packet format will be documented separately in:

`docs/communication/communication-architecture.md`

---

# 7. UART and Future SPI Evaluation

V1 will initially use:

```text
ESP32 RX → UART → STM32
```

UART is selected initially because it is:

- Simple
- Easy to debug
- Widely supported
- Sufficient for the initial architecture

SPI may later be evaluated between the ESP32 receiver and STM32.

The comparison will consider:

- Latency
- Reliability
- Implementation complexity
- CPU overhead
- Debugging difficulty
- Throughput

The communication interface can therefore be upgraded without changing the overall system architecture.

---

# 8. Flight Control Architecture

The STM32 flight-control pipeline is:

```text
Sensor Input
     |
     v
Sensor Calibration
     |
     v
Sensor Validation
     |
     v
State Estimation
     |
     v
Flight Mode Manager
     |
     v
Flight Controller
     |
     v
PID Control
     |
     v
Motor Mixing
     |
     v
ESC Output
     |
     v
Motors
```

The system will first achieve basic stabilization before additional autonomous functions are enabled.

The control architecture is intentionally layered so that future autonomous systems can generate desired flight commands without directly controlling the motors.

---

# 9. IMU

The IMU is the primary sensor for attitude stabilization.

It provides measurements used to determine the drone's motion and orientation.

The flight controller will use:

- Gyroscope data
- Accelerometer data

for stabilization.

Sensor calibration is required before reliable flight.

The system must validate sensor data before using it for flight control.

---

# 10. Magnetometer

A magnetometer may be used for heading-related functions where appropriate.

However, magnetic interference from:

- Motors
- ESCs
- High-current wiring
- Battery wiring

must be considered.

Therefore, magnetometer use will not be treated as a fundamental requirement for basic V1 stabilization.

Basic flight must remain possible without depending on the magnetometer.

If a magnetometer is used, its placement and calibration will be evaluated experimentally.

---

# 11. Downward ToF Sensor

A downward-facing Time-of-Flight (ToF) sensor may be used for:

- Ground-distance measurement
- Low-altitude awareness
- Controlled landing
- Landing verification

The ToF sensor is particularly useful for low-altitude behaviour.

The system must validate ToF readings before using them for automatic decisions.

ToF is not required for the fundamental attitude-stabilization loop.

---

# 12. GPS

GPS may be included as an optional V1 component.

GPS is not required for:

- Basic stabilization
- Manual flight
- Basic hovering
- Fundamental attitude control

GPS may later support:

- Outdoor positioning
- Position-related features
- Return-to-home
- Autonomous navigation

The architecture must remain functional when GPS is unavailable.

---

# 13. OLED Diagnostics

A 0.96-inch OLED will provide basic system feedback.

Possible information includes:

- Communication status
- Flight mode
- Arm state
- Battery status
- Sensor status
- GPS status
- System state
- Failsafe state
- Fault information
- Startup status

Example:

```text
+----------------------+
|    GANDABERUNDA      |
|----------------------|
| LINK : OK            |
| MODE : MANUAL        |
| ARM  : OFF           |
| BAT  : NORMAL        |
| IMU  : OK            |
| GPS  : OK            |
| SYS  : READY         |
+----------------------+
```

The OLED is primarily a diagnostic and operator-feedback interface.

---

# 14. Flight Modes

V1 will initially support two primary control modes:

```text
+----------------------+
| MANUAL               |
|                      |
| Controller commands  |
| provide the desired  |
| pilot input.         |
+----------------------+

+----------------------+
| AUTONOMOUS           |
|                      |
| The flight controller|
| executes predefined  |
| automatic behaviour. |
+----------------------+
```

A physical switch on the controller will select the operating mode.

The exact autonomous functions will be introduced only after reliable manual flight has been achieved.

---

# 15. Manual / Autonomous Mode Switching

The mode-selection switch determines whether the system receives its desired flight behaviour from:

- Manual controller input
- Autonomous control logic

The drone must not abruptly change control behaviour during a mode transition.

When the mode is changed, the system will enter a short controlled transition state.

During this transition:

- The current state is stabilized
- The new control source is prepared
- Sudden control discontinuities are avoided

The exact transition duration will be determined experimentally.

---

# 16. Arming System

The drone will use a dedicated ARM / DISARM control.

Arming is the deliberate action that permits the flight controller to enable motor operation.

Disarming prevents motor operation.

The system must not automatically arm after:

- Power-up
- Reset
- Communication recovery
- Flight-controller reboot

Normal startup requires deliberate operator action.

---

# 17. Startup Behaviour

When the drone powers on:

```text
POWER ON
   |
   v
SYSTEM INITIALIZATION
   |
   v
SENSOR INITIALIZATION
   |
   v
CALIBRATION / VALIDATION
   |
   v
SYSTEM CHECK
   |
   v
DISPLAY STATUS
   |
   v
DISARMED
   |
   v
WAIT FOR OPERATOR ARM COMMAND
```

The OLED will provide visual feedback during startup.

An audio indicator may also be used where practical.

The drone should clearly indicate when it is:

- Starting
- Initializing
- Ready
- Armed
- Disarmed
- In a fault state

---

# 18. Reset / Reboot During Flight

A flight-controller reset during flight must not be treated exactly like a normal ground startup.

The system should prioritize:

1. Fast recovery
2. Re-establishing valid control information
3. Maintaining stabilization where possible
4. Avoiding unnecessary delays
5. Preventing accidental re-arming

After reboot, the system will evaluate available controller communication using:

- Packet identity
- Packet timing
- Sequence information
- Command validity

A reboot must never result in unintended motor activation.

The detailed recovery strategy will be implemented and tested separately.

---

# 19. Failsafe Philosophy

Gandaberunda V1 will use a multi-condition failsafe architecture.

The flight controller continuously evaluates important system conditions rather than relying on a single failure flag.

Important conditions include:

- Communication state
- Packet freshness
- Battery state
- Sensor validity
- Flight mode
- Controller state
- Landing sensor state
- System faults

The final response is determined from the combined system state.

The exact thresholds will be determined through testing.

---

# 20. Communication Loss

If communication is disrupted:

```text
VALID COMMANDS
      |
      v
COMMUNICATION LOST
      |
      v
SHORT TOLERANCE PERIOD
      |
      v
STABILIZE / HOLD
      |
      v
CHECK COMMUNICATION
      |
      +--------------------+
      |                    |
 SIGNAL RETURNS        SIGNAL STILL LOST
      |                    |
      v                    v
RESUME CONTROL       CONTROLLED RESPONSE
```

The exact timeout and response will depend on the flight state and will be determined during testing.

The system should avoid both:

- Immediate unnecessary intervention
- Excessive waiting without valid commands

The goal is predictable behaviour during temporary communication interruptions.

---

# 21. Battery Condition

Battery monitoring will be available in V1.

Battery condition can participate in the failsafe decision system.

However, advanced battery optimization is intentionally outside the primary V1 scope.

The immediate V1 objective is safe and predictable operation rather than maximum flight endurance.

Battery-related thresholds will be experimentally validated before being used as automatic triggers.

---

# 22. Abnormal Motor Behaviour

Motor behaviour will be monitored as part of the safety architecture where practical.

If abnormal motor behaviour is detected, the system should prioritize:

1. Maintaining controllability
2. Stabilization
3. Entering an appropriate failsafe state
4. Controlled landing or shutdown when necessary

The exact detection strategy will be developed after the basic flight-control system is functional.

Motor fault detection is not required before the first basic stabilization tests.

---

# 23. Emergency Kill

A separate emergency-kill control will exist independently from normal ARM / DISARM control.

Purpose:

> Provide the operator with a direct emergency shutdown mechanism.

The emergency-kill mechanism is intended for situations where continued motor operation presents an immediate safety risk.

Because immediate motor shutdown during flight can itself result in a crash, the emergency-kill function must be carefully tested and clearly documented.

The emergency-kill control must not be accidentally triggered by normal control inputs.

---

# 24. Motor Control

V1 will initially use PWM-based ESC control.

```text
STM32
  |
  | PWM
  v
ESC
  |
  v
Brushless Motor
```

DShot will be evaluated later as an upgrade.

Initial priority:

- Understand PWM
- Understand ESC operation
- Verify motor control
- Verify motor direction
- Verify motor mixing
- Establish reliable output
- Achieve stable flight

Only after PWM-based control is reliable will DShot be introduced.

---

# 25. Motor Mixing

The flight controller converts desired:

- Throttle
- Roll
- Pitch
- Yaw

commands into individual motor outputs.

Conceptually:

```text
Desired Flight Commands
          |
          v
+----------------------+
|   Flight Controller  |
+----------------------+
          |
          v
     Motor Mixer
          |
     +----+----+----+
     |    |    |    |
     v    v    v    v
    M1   M2   M3   M4
```

The exact motor-mixing matrix will be documented separately.

---

# 26. Control Hierarchy

The V1 control system will follow a layered structure:

```text
Operator Commands
        |
        v
Flight Mode Manager
        |
        v
Desired Attitude / Throttle
        |
        v
Attitude Controller
        |
        v
PID Controllers
        |
        v
Motor Mixer
        |
        v
ESC Outputs
```

This structure allows future autonomous systems to generate desired flight commands without directly controlling motors.

---

# 27. Safety Priority

The system follows this conceptual priority:

```text
EMERGENCY SAFETY
       |
       v
FAILSAFE
       |
       v
STABILIZATION
       |
       v
FLIGHT MODE
       |
       v
CONTROL COMMAND
       |
       v
MOTOR OUTPUT
```

Safety-related decisions take precedence over normal user commands when necessary.

The flight controller remains the final authority over motor operation.

---

# 28. V1 Development Sequence

The system will not be developed as one large integration task.

Development will progress through:

```text
1. Learn fundamentals
        |
        v
2. Define requirements
        |
        v
3. Architecture
        |
        v
4. Component selection
        |
        v
5. Individual hardware testing
        |
        v
6. Sensor bring-up
        |
        v
7. Communication testing
        |
        v
8. ESC / motor testing
        |
        v
9. Flight-controller firmware
        |
        v
10. Calibration
        |
        v
11. Stabilization
        |
        v
12. Basic manual flight
        |
        v
13. Stable hover
        |
        v
14. Safety-system validation
        |
        v
15. Additional V1 features
        |
        v
16. Reliability testing
        |
        v
17. V1 release
```

Advanced features will not be added before basic flight is reliable.

---

# 29. Feature Integration Strategy

Gandaberunda V1 will follow a layered integration strategy.

The first milestone is:

```text
Stable Basic Flight
```

Only after this is achieved will additional features be introduced.

The general sequence is:

```text
Basic Flight
     |
     v
Stable Hover
     |
     v
Communication Reliability
     |
     v
Safety Features
     |
     v
OLED Diagnostics
     |
     v
Autonomous Behaviour
     |
     v
Additional Sensors
```

This prevents advanced features from making basic flight debugging unnecessarily difficult.

---

# 30. Upgrade Strategy

The V1 architecture is intentionally modular.

Future upgrades should be possible without replacing the complete system.

Potential upgrade areas include:

- DShot
- SPI communication
- RTOS
- Improved positioning
- Thermal camera
- Night vision
- Additional sensors
- Companion computer
- Computer vision
- Autonomous navigation
- Additional sensing payloads

The V1 architecture should provide suitable interfaces for future additions wherever practical.

---

# 31. RTOS Strategy

An RTOS is not part of the initial V1 firmware architecture.

The first version will use a simpler firmware structure so that both developers can understand:

- Timing
- Interrupts
- Drivers
- Sensor acquisition
- Control loops
- Communication
- State machines

Once the basic platform is reliable, an RTOS can be introduced as a separate engineering upgrade.

The RTOS migration should not be allowed to complicate the initial flight-control development unnecessarily.

---

# 32. Modularity Principle

Subsystems should communicate through clearly defined interfaces.

For example:

```text
Communication
      |
      v
Command Interface
      |
      v
Flight Controller
      |
      v
Motor Interface
```

This allows individual components to be replaced without redesigning the entire system.

Examples include:

```text
UART → SPI
PWM → DShot
Basic firmware → RTOS
Basic sensing → Advanced sensing
GPS → Improved positioning
```

The higher-level architecture should remain stable wherever possible.

---

# 33. V1 Boundary

The following are V1 priorities:

- Stable flight
- Manual control
- Custom controller
- Basic hovering
- Sensor calibration
- Communication reliability
- Safety systems
- OLED diagnostics
- Basic autonomous behaviour
- Modular firmware
- Documentation
- Testing and verification

The following are intentionally deferred to later versions:

- Advanced autonomous navigation
- Computer vision
- Thermal sensing
- Night vision
- Advanced battery optimization
- RTOS
- Advanced research sensing
- Complex autonomous missions

These features may be introduced after the V1 platform is proven reliable.

---

# 34. Engineering Principles

Gandaberunda follows these principles.

## Understand Before Implementing

Every major subsystem should be understood before integration.

## Test Before Flight

Hardware and software should be tested progressively before flight testing.

## Fail Safely

The system should have predictable behaviour when something goes wrong.

## Keep V1 Simple

Complexity should only be introduced when it provides clear engineering value.

## Design for Upgrades

V1 should not unnecessarily block future development.

## Both Developers Understand the System

Knowledge should not become permanently isolated between team members.

Both developers should understand the complete system, even when one developer is primarily responsible for a particular implementation task.

## Document Decisions

Important engineering decisions should be recorded together with their reasoning and trade-offs.

---

# 35. Related Documentation

Detailed information will be maintained in:

- [`Hardware Overview`](../hardware/hardware-overview.md)
- [`Software Architecture`](../software/software-architecture.md)
- [`Communication Architecture`](../communication/communication-architecture.md)
- [`Safety Architecture`](../safety/safety-architecture.md)
- [`Testing Strategy`](../testing/test-strategy.md)
- [`Engineering Decisions`](../decisions/engineering-decisions.md)
- [`Version Roadmap`](../versions/roadmap.md)

---

# 36. Architecture Status

**Current Status:** V1 Architecture — Defined

This document represents the current high-level design.

Hardware specifications, exact interfaces, timing values, control parameters, and implementation details may change during testing.

Any significant architectural change should be recorded in:

`docs/decisions/engineering-decisions.md`

The architecture should be reviewed whenever a major subsystem is changed.

---

> **Build the simplest reliable system first. Understand it completely. Then upgrade it.**