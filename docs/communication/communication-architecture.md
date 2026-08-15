# Project Gandaberunda — V1 Communication Architecture

> **Purpose:** Define how commands and status information move between the custom controller and the drone.

---

## 1. Communication Goal

The communication system must provide:

- Reliable pilot commands
- Predictable timing
- Packet validation
- Communication-loss detection
- Basic status feedback
- A clear boundary between communication and flight control

The communication system must **not directly control the motors**.

The STM32 remains the final authority over flight behaviour.

---

## 2. Communication Architecture

```text
                 CUSTOM CONTROLLER
                ┌──────────────────┐
                │      ESP32       │
                │                  │
                │ Joysticks        │
                │ Switches         │
                │ ARM / DISARM     │
                │ Flight Mode      │
                └────────┬─────────┘
                         │
                      ESP-NOW
                         │
                         ▼
                ┌──────────────────┐
                │   DRONE ESP32    │
                │                  │
                │ Receive          │
                │ Validate         │
                │ Monitor timing   │
                └────────┬─────────┘
                         │
                        UART
                         │
                         ▼
                ┌──────────────────┐
                │      STM32       │
                │                  │
                │ Command Manager  │
                │ Safety           │
                │ Flight Control   │
                └──────────────────┘
```

---

## 3. Communication Responsibilities

### Controller ESP32

Responsible for:

- Reading pilot inputs
- Converting inputs into commands
- Creating command packets
- Sending packets
- Displaying communication status

It does **not** calculate motor outputs.

---

### Drone ESP32

Responsible for:

- Receiving packets
- Checking packet integrity
- Checking packet sequence
- Monitoring packet timing
- Forwarding valid commands to the STM32

It acts primarily as a communication bridge.

---

### STM32

Responsible for:

- Receiving commands
- Validating commands
- Applying safety rules
- Deciding whether commands are accepted
- Running flight control
- Generating motor outputs

---

## 4. Communication Direction

### Primary direction

```text
Controller
    ↓
Drone
```

Pilot commands travel from the controller to the drone.

### Future status direction

```text
Drone
    ↓
Controller
```

The drone may return information such as:

- Battery status
- ARM state
- Flight mode
- Sensor status
- Communication status
- Fault information

---

## 5. V1 Wireless Protocol

The controller and drone ESP32 communicate using:

**ESP-NOW**

Reasons for using ESP-NOW:

- Direct ESP32-to-ESP32 communication
- Low overhead
- No Wi-Fi router required
- Suitable for short-range control
- Useful for learning wireless embedded communication

ESP-NOW is treated as the transport mechanism.

The flight-control system remains independent of it.

---

## 6. Wired Protocol

The initial interface between the drone ESP32 and STM32 is:

**UART**

```text
ESP32 RX
   │
   │ UART
   ▼
STM32
```

UART is being used initially because it is:

- Simple
- Easy to debug
- Widely supported
- Easy to observe with a logic analyzer or serial tools

SPI may be evaluated later.

---

## 7. Command Packet

The controller should send a structured packet.

Conceptually:

```text
┌──────────────────────────────┐
│ Packet Header                │
├──────────────────────────────┤
│ Packet Type                  │
├──────────────────────────────┤
│ Sequence Number              │
├──────────────────────────────┤
│ Timing Information           │
├──────────────────────────────┤
│ Throttle                     │
├──────────────────────────────┤
│ Roll                         │
├──────────────────────────────┤
│ Pitch                        │
├──────────────────────────────┤
│ Yaw                          │
├──────────────────────────────┤
│ Flight Mode                  │
├──────────────────────────────┤
│ ARM / DISARM                 │
├──────────────────────────────┤
│ Safety / Emergency State     │
├──────────────────────────────┤
│ Integrity Check              │
└──────────────────────────────┘
```

The exact binary packet format will be finalized during implementation.

---

## 8. Sequence Number

Each command packet should contain a sequence number.

Example:

```text
1001
1002
1003
1004
1005
```

The receiver can use this to determine whether:

- A packet is new
- A packet is duplicated
- Packets were missed
- Packets arrived out of order

This provides useful information without requiring complex communication logic.

---

## 9. Packet Timing

Communication timing is important for flight control.

The receiver should monitor:

```text
Packet
  ↓
Arrival Time
  ↓
Compare with previous packet
  ↓
Determine communication health
```

The system should distinguish between:

- Normal packet arrival
- Delayed packet
- Missing packet
- Communication loss

Exact timing thresholds will be determined during testing.

---

## 10. Packet Validation

A packet should not be accepted simply because it arrived.

The receiving system should check:

```text
Packet received
      ↓
Correct structure?
      ↓
Correct packet type?
      ↓
Valid sequence?
      ↓
Valid timing?
      ↓
Valid command ranges?
      ↓
Integrity check?
      ↓
ACCEPT
```

If validation fails:

```text
REJECT
```

Invalid data must not be passed into the flight controller.

---

## 11. Command Range Validation

Control values must remain within defined limits.

For example:

```text
Throttle → allowed range
Roll     → allowed range
Pitch    → allowed range
Yaw      → allowed range
```

Values outside the expected range should be rejected or safely constrained.

The exact ranges will be defined in the command interface specification.

---

## 12. Communication Timeout

The drone continuously monitors the time since the last valid command.

Conceptually:

```text
Last valid packet
       │
       ▼
Timer running
       │
       ├── Packet arrives
       │      ↓
       │    Reset timer
       │
       └── No packet
              ↓
        Timeout threshold
              ↓
           FAILSAFE
```

The exact timeout value will be selected through testing.

---

## 13. Communication Failure

Communication failure may occur because of:

- Controller power loss
- Drone receiver failure
- Wireless interference
- Controller software failure
- Antenna problems
- Packet corruption
- Excessive delay

The communication layer should report the failure to the STM32.

The STM32 then determines the appropriate safety response.

---

## 14. Communication vs Flight Control

This separation is fundamental.

```text
COMMUNICATION
     │
     │ Desired pilot command
     ▼
FLIGHT CONTROLLER
     │
     │ Safe control output
     ▼
MOTORS
```

The communication system says:

> "The pilot wants this."

The flight controller decides:

> "What should the motors actually do?"

---

## 15. ARM / DISARM Communication

ARM and DISARM are explicit commands.

Example:

```text
Controller
    │
    ▼
ARM command
    │
    ▼
Communication validation
    │
    ▼
STM32
    │
    ▼
Arming checks
    │
    ├── Failed → Remain DISARMED
    │
    └── Passed → ARM
```

The communication packet alone must never force the STM32 to arm.

---

## 16. Emergency Command

The controller should provide an emergency command.

Conceptually:

```text
Emergency Command
       ↓
STM32
       ↓
Safety Logic
       ↓
Emergency Response
```

The emergency command receives higher priority than normal flight commands.

The exact emergency behaviour will be defined in the safety architecture.

---

## 17. Communication States

The communication subsystem may use states such as:

```text
OFFLINE
   ↓
INITIALIZING
   ↓
SEARCHING
   ↓
CONNECTED
   ↓
DEGRADED
   ↓
LOST
```

The exact state transitions will be finalized during implementation.

---

## 18. Startup Communication

Normal startup:

```text
Controller ON
      ↓
Drone ON
      ↓
ESP32 initialization
      ↓
Communication established
      ↓
Valid packets received
      ↓
System READY
```

Communication availability does not automatically mean the drone is armed.

---

## 19. Recovery After Communication Loss

When communication is lost:

```text
Communication Loss
        ↓
STM32 detects failure
        ↓
Failsafe
        ↓
Safe response
```

When communication returns:

```text
Communication restored
        ↓
Validate new packets
        ↓
Restore normal command processing
```

Communication recovery must not automatically cause unintended arming.

---

## 20. Recovery After System Reset

A system reset is different from normal communication loss.

After reset:

```text
Reset
  ↓
Initialize
  ↓
Validate sensors
  ↓
Validate communication
  ↓
Enter safe state
```

The system must not assume that the previous flight state is automatically valid.

The detailed recovery strategy will be developed during safety testing.

---

## 21. Status Feedback

The drone may provide status information to the controller.

Possible information:

```text
LINK       : OK
BATTERY    : NORMAL
ARM        : OFF
MODE       : MANUAL
IMU        : OK
GPS        : OK
FAULT      : NONE
```

Status feedback is primarily intended for operator awareness and debugging.

---

## 22. Communication Data Flow

Complete V1 command flow:

```text
Pilot
  ↓
Controller Input
  ↓
Input Processing
  ↓
Command Packet
  ↓
ESP-NOW
  ↓
Drone ESP32
  ↓
Packet Validation
  ↓
UART
  ↓
STM32
  ↓
Command Validation
  ↓
Safety Checks
  ↓
Flight Controller
  ↓
Motor Mixer
  ↓
ESCs
  ↓
Motors
```

---

## 23. Communication Timing Philosophy

The communication system should prioritize:

1. Predictable timing
2. Valid data
3. Fresh data
4. Low unnecessary latency
5. Simplicity

We should not add complicated networking features unless they provide a measurable benefit.

---

## 24. UART vs SPI

### UART

Advantages:

- Simple
- Easy to debug
- Low complexity
- Widely supported

Disadvantages:

- Lower flexibility than SPI
- Communication speed is more limited

### SPI

Advantages:

- Higher data rate
- Synchronous communication
- Useful for future expansion

Disadvantages:

- More complex
- Requires additional implementation
- More sensitive to wiring/design details

### V1 decision

Use:

**UART first.**

Evaluate SPI later based on actual measurements.

---

## 25. Communication Security

V1 primarily focuses on reliable control rather than advanced cybersecurity.

However, the system should still avoid accepting arbitrary invalid data.

Basic protection includes:

- Packet validation
- Sequence checking
- Range checking
- Integrity checking
- Communication timeout
- Explicit command types

Advanced authentication/encryption can be considered in later versions if required.

---

## 26. Communication Testing

Testing will progress from simple to complex:

```text
ESP32 → ESP32
      ↓
Packet Transmission
      ↓
Packet Validation
      ↓
UART
      ↓
ESP32 → STM32
      ↓
End-to-End Commands
      ↓
Communication Loss
      ↓
Recovery
      ↓
Full System
```

Testing should include:

- Normal communication
- Missing packets
- Delayed packets
- Invalid packets
- Duplicate packets
- Out-of-order packets
- Communication loss
- Communication recovery
- Controller restart
- Receiver restart

---

## 27. Debugging

When communication fails, debugging should proceed in order:

```text
Power
 ↓
ESP32 startup
 ↓
Wireless connection
 ↓
Packet transmission
 ↓
Packet reception
 ↓
Packet validation
 ↓
UART transmission
 ↓
UART reception
 ↓
STM32 processing
 ↓
Flight-control response
```

Do not immediately change the entire communication system.

Find the first point where expected data stops appearing.

---

## 28. Future Communication Expansion

The architecture leaves room for:

- SPI
- Improved telemetry
- Additional status packets
- Companion computers
- Additional wireless links
- Advanced telemetry
- Autonomous mission communication

These should be added without coupling them directly to the motor-control system.

---

## 29. V1 Communication Boundary

### Required

- ESP32 controller
- ESP32 drone receiver
- ESP-NOW
- UART
- Command packets
- Sequence information
- Packet validation
- Timing monitoring
- Communication timeout
- ARM / DISARM
- Emergency command
- Basic status information

### Future

- SPI
- Advanced telemetry
- Advanced authentication
- Additional communication links
- Companion-computer communication

---

## 30. Final Principle

> **Communication carries intent. The STM32 decides how that intent can safely become motion.**

The V1 communication system should therefore remain:

**Simple → Predictable → Validated → Observable → Safe → Upgradeable**

---

**Status:** V1 Communication Architecture — Initial Baseline