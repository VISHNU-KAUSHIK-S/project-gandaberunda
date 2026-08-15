# Project Gandaberunda — Engineering Decisions

> **Purpose:** Record important engineering decisions made during the development of Project Gandaberunda.

---

# 1. Why This Document Exists

Engineering decisions should not live only in conversations or in our memory.

This document records:

- What we decided
- Why we decided it
- Alternatives considered
- Important trade-offs
- Whether the decision can be changed later

The purpose is not to prove that every decision is perfect.

The purpose is to make our reasoning visible.

---

# 2. Decision Format

Each major decision should follow:

```text
Decision
Why
Alternatives
Trade-offs
Status
```

Future decisions should be added to this document instead of silently changing the architecture.

---

# 3. ED-001 — Modular Architecture

### Decision

Gandaberunda will use a modular hardware and software architecture.

### Why

The project is intended to evolve beyond V1.

Future versions may include:

- Additional sensors
- Thermal sensing
- Night vision
- Companion computing
- Autonomous navigation
- Advanced sensing research

A modular design reduces the need to redesign the entire system.

### Alternatives

A single tightly integrated design.

### Trade-offs

Advantages:

- Easier upgrades
- Easier debugging
- Better learning value
- Easier replacement of failed modules

Disadvantages:

- More interfaces
- More documentation
- Slightly higher initial complexity

### Status

**Accepted**

---

# 4. ED-002 — STM32 as Flight Controller

### Decision

An STM32 microcontroller will be used as the primary flight controller for V1.

### Why

The flight controller requires:

- Predictable timing
- Hardware timers
- PWM generation
- Sensor interfaces
- Real-time control
- Low-level embedded programming

STM32 provides a strong platform for learning and implementing these concepts.

### Alternatives

- Arduino-class microcontrollers
- ESP32 as the primary flight controller
- Ready-made commercial flight controller

### Trade-offs

Advantages:

- Strong embedded learning value
- Good real-time capabilities
- Large ecosystem
- Suitable peripherals
- Good future expansion potential

Disadvantages:

- Higher learning curve
- More firmware development
- More responsibility for implementation

### Status

**Accepted**

---

# 5. ED-003 — ESP32 for Wireless Communication

### Decision

ESP32 will be used for the custom controller and drone-side wireless communication.

### Why

ESP32 provides:

- Wireless connectivity
- ESP-NOW
- Sufficient processing power
- Low cost
- Good development support

It allows the wireless communication system to remain separate from the flight-control processor.

### Alternatives

- Wi-Fi
- Bluetooth
- Dedicated RF modules
- ESP32 as the complete flight controller

### Trade-offs

Advantages:

- Low cost
- Easy experimentation
- Wireless capability
- Separation between communication and flight control

Disadvantages:

- Additional hardware
- Additional firmware
- Wireless reliability must be tested

### Status

**Accepted**

---

# 6. ED-004 — ESP-NOW for V1 Wireless Control

### Decision

ESP-NOW will be used for controller-to-drone wireless communication in V1.

### Why

The project requires direct communication without depending on a Wi-Fi router.

ESP-NOW provides a relatively simple way to establish direct ESP32-to-ESP32 communication.

### Alternatives

- Wi-Fi
- Bluetooth
- Dedicated RF transceiver

### Trade-offs

Advantages:

- Simple architecture
- No router required
- Low overhead
- Good learning value

Disadvantages:

- Range and reliability must be experimentally verified
- Advanced networking features are not the primary focus

### Status

**Accepted for V1**

---

# 7. ED-005 — UART Between ESP32 and STM32

### Decision

UART will initially be used between the drone ESP32 and STM32.

### Why

UART is:

- Simple
- Easy to understand
- Easy to debug
- Widely supported

It allows us to establish a working communication path before experimenting with faster interfaces.

### Alternative

SPI.

### Trade-offs

UART:

- Simpler
- Easier debugging
- Lower complexity

SPI:

- Higher potential throughput
- More complex implementation
- More signals and timing considerations

### Status

**Accepted for V1**

SPI may be evaluated later based on measured requirements.

---

# 8. ED-006 — Communication Must Not Directly Control Motors

### Decision

The communication subsystem will send desired commands to the STM32.

It will never directly generate motor commands.

### Why

The STM32 must remain the final authority over:

- Safety
- Flight state
- Control calculations
- Motor outputs

This prevents communication failures from directly becoming motor-control failures.

### Architecture

```text
Controller
    ↓
Communication
    ↓
STM32
    ↓
Safety
    ↓
Flight Control
    ↓
Motor Output
```

### Status

**Accepted**

---

# 9. ED-007 — PWM for V1 ESC Control

### Decision

PWM will initially be used between the STM32 and ESCs.

### Why

PWM is simple and suitable for learning the complete control path.

### Alternative

DShot.

### Trade-offs

PWM:

- Simple
- Easy to understand
- Easy to test

DShot:

- Digital
- More advanced
- Better future potential
- Requires additional implementation

### Status

**Accepted for V1**

DShot may be evaluated later.

---

# 10. ED-008 — V1 Focuses on Basic Reliable Flight

### Decision

V1 will prioritize reliable basic flight over advanced features.

### V1 Goal

The drone should:

- Arm safely
- Respond predictably
- Stabilize itself
- Hover reliably
- Accept commands from the custom controller
- Handle normal communication loss safely

### Why

Adding advanced features before the flight platform is reliable increases debugging complexity.

### Status

**Accepted**

---

# 11. ED-009 — Features Added Only After Basic Flight

### Decision

Additional features will be added only after the basic flight system is reliable.

### Development order

```text
Hardware
   ↓
Calibration
   ↓
Sensor validation
   ↓
Communication
   ↓
Motor control
   ↓
Flight control
   ↓
Stable hover
   ↓
Basic flight
   ↓
Additional features
```

### Why

This creates a known-good baseline.

### Status

**Accepted**

---

# 12. ED-010 — Safety Before Performance

### Decision

Safety takes priority over performance.

### Why

A drone is a physical system capable of causing damage if it behaves unexpectedly.

The system should therefore prefer:

```text
Safe state
    >
Continued operation
```

when a critical fault occurs.

### Status

**Accepted**

---

# 13. ED-011 — Explicit ARM / DISARM

### Decision

The drone will use an explicit ARM / DISARM system.

### Why

Motor operation should never begin simply because the system has powered on.

### Rule

Startup:

```text
POWER ON
   ↓
INITIALIZE
   ↓
CHECK
   ↓
DISARMED
```

The system must only arm after the required checks succeed.

### Status

**Accepted**

---

# 14. ED-012 — Communication Failsafe

### Decision

The STM32 will monitor communication freshness and enter a defined failsafe state when communication is lost.

### Why

The drone must not continue indefinitely using outdated pilot commands.

### Monitoring

The system will use:

- Packet sequence information
- Packet timing
- Validity checks
- Timeout detection

### Status

**Accepted**

---

# 15. ED-013 — Packet Validation

### Decision

Incoming packets must be validated before being used.

### Validation includes

- Packet structure
- Packet type
- Command ranges
- Sequence information
- Timing
- Integrity

### Why

Wireless communication cannot be assumed to be perfect.

### Status

**Accepted**

---

# 16. ED-014 — Separate Communication and Flight Control

### Decision

Communication processing and flight-control processing will remain separate software modules.

### Why

This improves:

- Debugging
- Maintainability
- Testing
- Future protocol changes

### Status

**Accepted**

---

# 17. ED-015 — Sensor Drivers Separate From Flight Control

### Decision

Each sensor will have a dedicated driver/interface.

### Why

The flight-control code should not depend on the low-level details of a particular sensor.

This makes sensor replacement easier.

### Architecture

```text
Flight Control
      ↓
Sensor Interface
      ↓
Sensor Driver
      ↓
Physical Sensor
```

### Status

**Accepted**

---

# 18. ED-016 — Budget-Conscious Development

### Decision

Hardware spending will be minimized without compromising safety or essential reliability.

### Principles

- Avoid unnecessary purchases
- Prefer reusable hardware
- Prefer widely available components
- Avoid buying advanced hardware before it is needed
- Keep budget for future upgrades
- Consider total system cost
- Consider weight and power

### Why

The project is being developed with a student budget.

The goal is:

> Maximum engineering value per rupee.

### Status

**Accepted**

---

# 19. ED-017 — V1 Hardware Must Support Future Upgrades

### Decision

V1 hardware should not unnecessarily prevent future expansion.

### Future possibilities

- Thermal sensing
- Night vision
- Additional sensors
- Companion computing
- Autonomous navigation
- Advanced research payloads

### Why

V1 is a foundation rather than the final product.

### Status

**Accepted**

---

# 20. ED-018 — Both Developers Learn the Complete System

### Decision

Both developers will understand all major parts of the project.

### This includes

- Hardware
- Embedded programming
- Communication
- Sensors
- Flight control
- PID
- Safety
- Testing
- Documentation

### Why

The objective is to become capable engineers, not simply divide the project into isolated assignments.

### Working method

Both developers may own different modules temporarily, but ownership does not mean exclusive knowledge.

### Status

**Accepted**

---

# 21. ED-019 — Documentation Is Part of Development

### Decision

Documentation will be maintained throughout development.

### Required documentation includes

- Architecture
- Hardware
- Software
- Communication
- Safety
- Testing
- Engineering decisions
- Version roadmap

### Why

Documentation improves:

- Understanding
- Debugging
- Team communication
- Reproducibility
- Future upgrades

### Status

**Accepted**

---

# 22. ED-020 — Test Before Integration

### Decision

Subsystems should be tested individually before complete system integration.

### Example

```text
Sensor
   ↓
Sensor Driver
   ↓
Sensor Module
   ↓
Flight Controller
   ↓
Complete System
```

### Why

Testing everything simultaneously makes failures difficult to isolate.

### Status

**Accepted**

---

# 23. ED-021 — Do Not Blindly Copy PID Values

### Decision

PID parameters will be developed and tuned based on our own system.

### Why

PID behaviour depends on:

- Vehicle mass
- Motors
- Propellers
- Frame
- Sensor behaviour
- Control loop timing
- Mechanical vibration

Values from another drone may not behave correctly on Gandaberunda.

### Status

**Accepted**

---

# 24. ED-022 — Experimental Features Stay Outside the Critical Flight Path

### Decision

Experimental features should not be allowed to destabilize the basic flight-control system.

### Example

Future research systems should be separated from:

```text
Core Flight Control
```

where practical.

### Why

Research features may change frequently and may not always be reliable.

### Status

**Accepted**

---

# 25. ED-023 — Change One Major Variable at a Time During Debugging

### Decision

When diagnosing a problem, changes should be made systematically.

### Method

```text
Observe
   ↓
Identify failure
   ↓
Form hypothesis
   ↓
Change one relevant variable
   ↓
Test
   ↓
Compare result
```

### Why

Changing many things simultaneously makes the root cause difficult to identify.

### Status

**Accepted**

---

# 26. ED-024 — Development Before Flight

### Decision

No subsystem will be considered flight-ready merely because it compiles or appears to work on the bench.

### Flight readiness requires:

- Testing
- Verification
- Safety checks
- Integration testing
- Controlled ground testing
- Documented results

### Status

**Accepted**

---

# 27. Decision Review

Engineering decisions are not permanent laws.

A decision may be reconsidered when:

- New requirements appear
- Testing provides new evidence
- A component becomes unavailable
- A better technical solution is found
- Cost changes significantly
- Safety concerns are discovered

When a decision changes:

1. Record the new decision.
2. Explain why it changed.
3. Record the old decision.
4. Update affected architecture documents.

---

# 28. Future Decision Log

New decisions should be added below this section.

Use:

```text
ED-XXX — Decision Title

Decision:

Why:

Alternatives:

Trade-offs:

Status:
```

---

# Final Principle

> **Good engineering is not about always making the perfect decision. It is about making informed decisions, testing them, documenting them, and being willing to change them when evidence says we should.**

---

**Status:** Active Engineering Decision Log