# Project Gandaberunda — V1 Safety Architecture

> **Purpose:** Define the safety mechanisms, safe states, failsafes, and shutdown behaviour of Gandaberunda V1.

---

## 1. Safety Philosophy

Safety is more important than flight performance.

The system should always prefer:

**Safe behaviour > continued operation**

Safety must be implemented in both hardware and software.

No single software feature should be trusted as the only protection against an unsafe condition.

---

## 2. Safety Architecture

```text
                 PILOT
                   │
                   ▼
              CONTROLLER
                   │
                   ▼
            COMMUNICATION
                   │
                   ▼
             ESP32 RECEIVER
                   │
                   ▼
                STM32
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
     SAFETY CHECKS      FLIGHT CONTROL
          │                 │
          └────────┬────────┘
                   │
                   ▼
              MOTOR OUTPUT
                   │
                   ▼
                 ESCs
                   │
                   ▼
                MOTORS
```

The safety system can prevent or override normal flight commands.

---

## 3. Safe State

The primary safe state for V1 is:

```text
DISARMED
```

In the safe state:

- Motors must not produce intentional flight thrust.
- Normal flight commands must not cause motor activation.
- The system may continue monitoring sensors and communication.

---

## 4. System States

The system should use explicit states.

```text
BOOT
  ↓
INITIALIZING
  ↓
CHECKING
  ↓
READY
  ↓
ARMED
  ↓
FLIGHT
  ↓
DISARMED
```

Fault conditions may move the system toward a safe state.

---

## 5. Startup Safety

After power-on:

```text
POWER ON
   ↓
INITIALIZATION
   ↓
SENSOR CHECK
   ↓
COMMUNICATION CHECK
   ↓
SYSTEM CHECK
   ↓
READY
```

The system must **not automatically arm** after startup.

The pilot must explicitly request arming.

---

## 6. Arming Conditions

Before arming, the STM32 should verify the required conditions.

Examples:

- Flight controller initialized
- IMU functioning
- Sensor data valid
- Communication available
- Battery acceptable
- Throttle in safe position
- No active critical fault
- System orientation/status acceptable

If a required condition fails:

```text
ARM REQUEST
     ↓
CHECKS FAILED
     ↓
REMAIN DISARMED
```

---

## 7. Disarming

Disarming should immediately remove normal flight authority from the flight controller.

Possible causes:

- Pilot command
- Emergency command
- Critical fault
- Communication failure
- System shutdown

The exact behaviour for each condition will be tested before flight.

---

## 8. Emergency Control

The controller should provide an emergency control.

Its purpose is to give the pilot a simple way to request an immediate safe response.

Conceptually:

```text
EMERGENCY
    ↓
COMMUNICATION
    ↓
STM32 SAFETY LOGIC
    ↓
SAFE RESPONSE
```

The emergency mechanism must not depend on normal flight-control calculations.

---

## 9. Communication Failsafe

The drone continuously monitors communication.

```text
Valid packets
     ↓
Normal operation
```

If valid packets stop arriving:

```text
No valid packet
      ↓
Timeout detection
      ↓
Communication failsafe
      ↓
Safe response
```

The exact timeout threshold will be determined through testing.

---

## 10. Stale Commands

A command that is too old should not be treated as a current pilot command.

The system should use packet timing information to determine whether a command is fresh.

```text
Fresh command → ACCEPT

Old command → REJECT
```

This prevents delayed commands from unexpectedly affecting flight.

---

## 11. Invalid Packets

Invalid packets must not enter the flight-control system.

Examples:

- Corrupted packet
- Invalid packet type
- Invalid command range
- Invalid integrity check
- Invalid sequence information

Response:

```text
Invalid packet
      ↓
Reject
      ↓
Keep previous safe state
```

If invalid communication continues long enough, the communication failsafe may be triggered.

---

## 12. Sensor Failure

The flight controller depends heavily on sensor data.

If a critical sensor becomes invalid:

```text
Sensor failure
      ↓
Safety layer
      ↓
Determine severity
      ↓
Safe response
```

The system should never silently treat obviously invalid sensor data as valid.

---

## 13. IMU Failure

The IMU is critical to stabilization.

If the IMU is unavailable or produces invalid data:

```text
IMU failure
    ↓
Flight control cannot be trusted
    ↓
Do not arm / enter safe state
```

The drone should not attempt normal stabilized flight without valid primary attitude information.

---

## 14. Battery Safety

Battery voltage should be monitored.

Conceptually:

```text
NORMAL
   ↓
WARNING
   ↓
CRITICAL
```

### Normal

Normal flight operation.

### Warning

Pilot is informed that battery voltage is becoming low.

### Critical

The system enters the predefined low-battery response.

Exact thresholds and actions will be determined using the selected battery and measured flight behaviour.

---

## 15. Motor Safety

Motor output must remain disabled while disarmed.

Before enabling motors:

- ARM state must be valid
- Safety checks must pass
- Throttle must be in the expected position
- No critical fault should be active

Motor output should be constrained to safe limits.

---

## 16. Propeller Safety

Propellers are a major physical hazard.

During development:

- Propellers should be removed whenever possible.
- Motor tests should initially be performed without propellers.
- Software tests should be performed without propellers.
- Wiring should be checked before installing propellers.
- Final motor direction must be verified before flight.

No software test requires people to be near spinning propellers.

---

## 17. Ground Testing Safety

Testing should progress gradually.

```text
Software only
     ↓
Hardware without propellers
     ↓
Motor testing without propellers
     ↓
Low-risk integrated testing
     ↓
Controlled flight testing
```

Never jump directly from code completion to free flight.

---

## 18. Motor Test Procedure

Before testing motors:

1. Remove propellers.
2. Secure the drone.
3. Verify battery connections.
4. Verify motor numbering.
5. Verify motor direction.
6. Verify ESC connections.
7. Verify emergency/disarm control.
8. Test at low output first.

Only after successful bench testing should propellers be installed.

---

## 19. Power Safety

Before connecting the main battery:

- Check polarity.
- Check wiring.
- Check connectors.
- Check for short circuits.
- Check regulator output.
- Check expected voltage levels.

Power should be introduced gradually during initial testing.

---

## 20. Hardware Shutdown

The system should provide a clear way to remove power.

At minimum:

```text
Battery
   ↓
Main Power Connection
   ↓
Power Distribution
```

The main power connection should be accessible and secure.

---

## 21. Software Watchdog

A watchdog may be used to detect firmware failures.

Conceptually:

```text
Firmware running normally
       ↓
Watchdog regularly serviced
```

If the firmware becomes stuck:

```text
Firmware failure
      ↓
Watchdog timeout
      ↓
System reset
```

After reset, the system must return to a safe state.

The watchdog must not be treated as a replacement for proper fault handling.

---

## 22. Fault Classification

Faults can be classified broadly.

### Level 1 — Informational

Examples:

- GPS unavailable
- Optional sensor unavailable

The system may continue normal operation if the sensor is not required.

### Level 2 — Warning

Examples:

- Low battery
- Degraded communication
- Sensor quality reduced

The pilot should be informed and the system may restrict operation.

### Level 3 — Critical

Examples:

- Critical IMU failure
- Severe communication loss
- Major firmware fault
- Unsafe sensor state

The system should enter an appropriate safe state.

---

## 23. Safety Priority

Safety decisions should have higher priority than normal flight commands.

```text
Emergency / Critical Fault
          ↓
      SAFETY LOGIC
          ↓
      FLIGHT CONTROL
          ↓
       MOTOR OUTPUT
```

Normal commands must never override a critical safety condition.

---

## 24. Failsafe Philosophy

A failsafe should be:

- Predictable
- Simple
- Testable
- Documented
- Easy to understand

We should avoid complicated failsafe behaviour until the basic system is thoroughly tested.

---

## 25. Communication Recovery

When communication returns after a loss:

```text
Communication restored
       ↓
Receive new packet
       ↓
Validate packet
       ↓
Check system state
       ↓
Resume normal operation only if safe
```

A restored communication link should not automatically cause an unsafe state transition.

---

## 26. Reset Recovery

If the STM32 or ESP32 resets:

```text
RESET
  ↓
INITIALIZATION
  ↓
SAFETY CHECKS
  ↓
DISARMED
  ↓
READY
```

The system must not assume that the previous flight state is still valid.

---

## 27. Safety Monitoring

The STM32 should monitor important conditions such as:

```text
Communication
Sensor status
Battery
ARM state
Flight mode
System faults
Loop health
```

The safety system should continuously evaluate these conditions during operation.

---

## 28. Safety Testing

Safety testing must be performed intentionally.

Examples:

### Communication

- Turn controller off.
- Interrupt communication.
- Introduce invalid packets.
- Introduce delayed packets.

### Sensors

- Disconnect a sensor during bench testing.
- Provide invalid sensor values.
- Simulate sensor failure where possible.

### Battery

- Test warning thresholds.
- Test critical thresholds.

### Firmware

- Test watchdog behaviour.
- Test reset behaviour.
- Test startup behaviour.

All tests must first be performed without propellers.

---

## 29. Safety Test Progression

```text
Unit Test
   ↓
Bench Test
   ↓
Integrated Test
   ↓
Controlled Motor Test
   ↓
Tethered / Controlled Flight Test
   ↓
Normal Flight
```

Each stage must pass before moving to the next stage.

---

## 30. Safety Rules

### Rule 1

Never test unknown motor behaviour with propellers installed.

### Rule 2

Never assume communication is reliable without testing it.

### Rule 3

Never assume a sensor is valid without checking it.

### Rule 4

Never automatically arm after startup or reset.

### Rule 5

Safety logic must be independent of normal pilot commands.

### Rule 6

Critical faults must have a defined response.

### Rule 7

Both developers must understand the safety system.

### Rule 8

Every safety mechanism must be tested.

### Rule 9

Do not add complicated failsafes before basic failsafes are reliable.

### Rule 10

When uncertain, choose the safer state.

---

## 31. V1 Safety Boundary

### Required

- ARM / DISARM
- Emergency control
- Communication timeout
- Packet validation
- Sensor validation
- Battery monitoring
- Safe startup
- Safe reset
- Motor-output limits
- Basic fault handling
- Watchdog
- Controlled testing procedure

### Future

- Advanced autonomous failsafes
- GPS-based emergency behaviour
- Advanced navigation recovery
- Redundant sensors
- More sophisticated fault detection

---

## 32. Final Principle

> **If the system cannot safely handle a failure, the feature is not ready for flight.**

V1 safety should be:

**Simple → Predictable → Testable → Conservative → Reliable**

---

**Status:** V1 Safety Architecture — Initial Baseline