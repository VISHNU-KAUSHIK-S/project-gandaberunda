# Project Gandaberunda — V1 Test Strategy

> **Purpose:** Define how Gandaberunda V1 will be tested, verified, and validated before flight.

---

## 1. Testing Philosophy

Testing is not the final step.

Testing happens throughout development.

We will follow:

```text
Build
  ↓
Test
  ↓
Measure
  ↓
Debug
  ↓
Improve
  ↓
Retest
```

A feature is not considered complete simply because the code compiles.

---

## 2. Testing Goals

Testing must answer:

- Does the hardware work?
- Does the software work?
- Do the subsystems communicate correctly?
- Does the system behave predictably?
- Does it fail safely?
- Can both developers reproduce and diagnose problems?
- Is the drone ready for the next level of testing?

---

## 3. Testing Levels

Testing will be performed at several levels:

```text
Unit Testing
     ↓
Module Testing
     ↓
Subsystem Testing
     ↓
System Testing
     ↓
Ground Testing
     ↓
Flight Testing
```

Each level builds confidence before moving to the next.

---

## 4. Unit Testing

Unit testing checks small pieces of software independently.

Examples:

- Packet parser
- Range checking
- Motor mixing calculations
- PID calculations
- State transitions
- Sensor conversion functions
- Battery calculations

The goal is to verify individual functions before combining them.

---

## 5. Module Testing

Module testing checks complete software modules.

Examples:

```text
Communication Module
Sensor Module
Safety Module
Motor Output Module
Controller Input Module
```

Each module should be tested independently wherever practical.

---

## 6. Hardware Testing

Hardware should initially be tested independently.

Examples:

- STM32 startup
- ESP32 startup
- IMU communication
- ToF communication
- GPS communication
- OLED
- Voltage regulator
- ESC
- Motor
- Battery monitoring

This prevents multiple unknowns from being introduced simultaneously.

---

## 7. Communication Testing

Communication tests should verify:

### Wireless

- Controller transmission
- Receiver reception
- Packet structure
- Sequence numbers
- Packet timing
- Invalid packets
- Missing packets
- Duplicate packets
- Delayed packets
- Communication loss

### UART

- ESP32 transmission
- STM32 reception
- Correct baud rate
- Packet framing
- Data integrity
- Error handling

---

## 8. Sensor Testing

Each sensor must be tested before being used in flight control.

### IMU

Verify:

- Accelerometer readings
- Gyroscope readings
- Axis orientation
- Calibration
- Noise
- Update rate

### ToF

Verify:

- Distance readings
- Valid range
- Update rate
- Invalid readings

### GPS

Verify:

- Fix acquisition
- Position output
- Update rate
- Invalid/no-fix behaviour

### Magnetometer

Verify:

- Axis readings
- Calibration
- Heading behaviour
- Magnetic interference

---

## 9. Motor and ESC Testing

Motor testing must initially be performed **without propellers**.

Test:

- ESC startup
- Motor numbering
- Motor direction
- Minimum output
- Maximum safe output
- Response to command changes

Example:

```text
STM32
  ↓
Motor Output
  ↓
ESC
  ↓
Motor
```

Every motor must respond to the correct output channel.

---

## 10. Propeller Testing

Before installing propellers:

- Motor direction must be confirmed.
- Motor numbering must be confirmed.
- Mixer direction must be confirmed.
- ARM / DISARM must be confirmed.
- Emergency control must be confirmed.

Propellers should only be installed after the motor system has passed bench testing.

---

## 11. Power Testing

Power testing must verify:

- Battery voltage
- Regulator output
- STM32 supply
- ESP32 supply
- Sensor supply
- Current behaviour
- Connector integrity
- Excessive heating

Initial power testing should be performed without motors whenever practical.

---

## 12. Flight Controller Testing

The flight controller will be tested progressively.

### Stage 1

Verify sensor readings.

### Stage 2

Verify sensor orientation.

### Stage 3

Verify state estimation.

### Stage 4

Verify control calculations.

### Stage 5

Verify motor mixing.

### Stage 6

Verify ESC outputs.

### Stage 7

Verify complete control loop.

Only after these stages should controlled flight testing begin.

---

## 13. PID Testing

PID development should progress gradually.

```text
No PID
  ↓
Understand response
  ↓
P control
  ↓
PI / PD experiments
  ↓
PID
  ↓
Tune
  ↓
Validate
```

Testing should examine:

- Stability
- Overshoot
- Oscillation
- Response time
- Noise sensitivity
- Recovery from disturbances

PID values must be documented.

---

## 14. Safety Testing

Safety functions must be tested intentionally.

Examples:

### ARM

- Attempt to arm with invalid conditions.
- Verify that arming is rejected.

### DISARM

- Verify normal disarm behaviour.

### Emergency

- Verify emergency response.

### Communication loss

- Stop controller transmission.
- Verify communication timeout.
- Verify failsafe response.

### Sensor failure

- Simulate or disconnect sensors during bench testing.
- Verify correct fault handling.

### Reset

- Reset the system.
- Verify that it returns to a safe state.

---

## 15. Ground Testing

Ground testing integrates the complete system without flight.

Example:

```text
Controller
    ↓
ESP-NOW
    ↓
Drone ESP32
    ↓
UART
    ↓
STM32
    ↓
Flight Controller
    ↓
ESC
    ↓
Motor
```

Initially this should be tested without propellers.

---

## 16. End-to-End Testing

The complete command path must be verified.

Example:

```text
Move controller joystick
        ↓
Controller reads input
        ↓
Packet generated
        ↓
ESP-NOW transmission
        ↓
Drone ESP32 receives packet
        ↓
Packet validated
        ↓
UART
        ↓
STM32 receives command
        ↓
Command validated
        ↓
Flight controller processes command
        ↓
Motor mixer generates outputs
        ↓
ESC receives output
```

Every stage should be observable during development.

---

## 17. Flight Testing

Flight testing must be gradual.

```text
Test Environment
      ↓
Controlled Ground Test
      ↓
First Lift
      ↓
Short Hover
      ↓
Stable Hover
      ↓
Basic Manoeuvres
      ↓
Longer Flight
```

Do not introduce multiple new features during the first flight tests.

---

## 18. First Flight Objective

The first flight is **not** a feature demonstration.

Its purpose is to verify:

- Motors behave correctly
- Control directions are correct
- Drone can lift safely
- Basic stabilization works
- Pilot commands behave predictably
- Drone can return to a controlled landing

Keep the first flight simple.

---

## 19. Flight Test Conditions

Before every flight:

### Hardware

- Battery secure
- Propellers correct
- Motors secure
- ESCs secure
- Wiring secure
- Sensors secure

### Software

- Correct firmware
- Correct configuration
- Correct motor mapping
- Correct flight mode
- Correct safety settings

### Environment

- Suitable open area
- No nearby people
- No obstacles
- Suitable weather
- Clear takeoff/landing area

---

## 20. Pre-Flight Checklist

```text
[ ] Frame inspected
[ ] Motors secure
[ ] Propellers correct
[ ] Battery secure
[ ] Wiring checked
[ ] STM32 powered correctly
[ ] ESP32 communication verified
[ ] IMU healthy
[ ] Controller operational
[ ] ARM / DISARM verified
[ ] Emergency control verified
[ ] Battery acceptable
[ ] Flight area clear
```

No flight should begin until the checklist is complete.

---

## 21. Test Results

Every important test should record:

```text
Test ID
Date
Software Version
Hardware Configuration
Test Objective
Test Procedure
Expected Result
Actual Result
Pass / Fail
Observations
Issues
Next Action
```

This creates a useful engineering history.

---

## 22. Test IDs

Use simple identifiers.

Example:

```text
HW-001
HW-002

SW-001
SW-002

COM-001
COM-002

SAFE-001
SAFE-002

FLT-001
FLT-002
```

Where:

```text
HW   = Hardware
SW   = Software
COM  = Communication
SAFE = Safety
FLT  = Flight
```

---

## 23. Pass / Fail Criteria

A test should have a clear expected result.

Example:

```text
Test:
Verify controller packet reception.

Expected:
Drone ESP32 receives valid packets continuously.

Result:
PASS / FAIL
```

Avoid subjective statements such as:

> "Seems to work."

Use measurable observations whenever possible.

---

## 24. Regression Testing

When a change is made, previously working features should be checked again.

Example:

```text
Change PID code
      ↓
Test PID
      ↓
Test sensors
      ↓
Test communication
      ↓
Test safety
      ↓
Test motor output
```

A new feature must not silently break an existing subsystem.

---

## 25. Debugging Method

When a test fails:

```text
1. Reproduce the failure
        ↓
2. Record what happened
        ↓
3. Identify the first incorrect behaviour
        ↓
4. Isolate the subsystem
        ↓
5. Check assumptions
        ↓
6. Change one thing
        ↓
7. Retest
        ↓
8. Document the result
```

Do not immediately rewrite large sections of code.

---

## 26. Instrumentation

Useful debugging tools may include:

- Serial output
- Logic analyzer
- Multimeter
- Oscilloscope
- USB interface
- Firmware logs
- Test scripts

Instrumentation should help answer:

> "What is actually happening?"

rather than simply producing large amounts of debug output.

---

## 27. Test Environment

Whenever practical, maintain separate environments:

```text
Development
     ↓
Bench Testing
     ↓
Controlled Flight Testing
```

Experimental firmware should not automatically become flight firmware.

---

## 28. Version Control and Testing

Every major test should record the software version.

Example:

```text
Firmware:
v0.1.0

Hardware:
V1 Prototype A

Test:
FLT-001
```

This makes it possible to reproduce previous results.

---

## 29. Two-Developer Verification

Both developers must understand and participate in testing.

Neither developer should become the only person who knows:

- How to test the flight controller
- How to diagnose communication
- How to tune PID
- How to perform safety checks
- How to operate the test procedure

The goal is shared engineering knowledge.

---

## 30. Test Progression

The overall V1 test progression is:

```text
Component
   ↓
Module
   ↓
Subsystem
   ↓
Integrated System
   ↓
Bench Test
   ↓
Motor Test
   ↓
Safety Test
   ↓
Controlled Flight
   ↓
Stable Hover
   ↓
Basic Flight
```

A failure at any stage sends the system back to debugging before progressing.

---

## 31. V1 Flight Readiness

V1 should not be considered flight-ready until:

- Sensors work reliably
- Communication works reliably
- Commands are validated
- ARM / DISARM works
- Emergency control works
- Failsafe works
- Motor mapping is verified
- Motor directions are verified
- Battery monitoring works
- Flight controller behaves predictably
- Basic stabilization works
- Controlled ground tests pass

---

## 32. Final Principle

> **We do not fly because the code compiles. We fly because the system has been tested and we understand its behaviour.**

The V1 testing process should be:

**Progressive → Measurable → Repeatable → Documented → Safe**

---

**Status:** V1 Test Strategy — Initial Baseline