---
topic: "Outline Topic 1 — Introduction to Embedded Systems"
source: "General embedded systems fundamentals — free reference: docs.espressif.com, standard embedded systems curricula"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material]
---

# Introduction to Embedded Systems

## 1. Definition
An **embedded system** is a computer system — a combination of a processor, memory, and I/O — built to perform one or a small set of **dedicated functions** within a larger mechanical or electrical system, usually with real-time constraints. Contrast with a general-purpose computer (PC/phone), which runs arbitrary user-chosen software.

Examples: a car's ABS controller, a washing machine's control board, a pacemaker, a Wi-Fi router, the ESP32 in a smart thermostat.

## 2. Key characteristics (very testable list)
| Characteristic | Meaning |
|---|---|
| **Dedicated function** | Designed for one specific task, not general computing |
| **Real-time constraints** | Must respond within a defined time window (hard vs soft real-time — see below) |
| **Resource-constrained** | Limited RAM/flash/CPU/power compared to general computers |
| **Reactive/reliable** | Often runs continuously for long periods (years), must handle faults gracefully |
| **Tightly coupled to hardware** | Software directly interacts with sensors/actuators/peripherals via registers |

## 3. Hard vs soft real-time systems
- **Hard real-time**: missing a deadline is a system failure (e.g. airbag deployment timing, anti-lock brakes). Correctness depends on both logical result AND timing.
- **Soft real-time**: missing a deadline degrades quality but isn't catastrophic (e.g. video streaming frame timing, sensor logging that's a few ms late).

## 4. Microcontroller vs Microprocessor (classic exam distinction)
| | Microcontroller (MCU) | Microprocessor (MPU) |
|---|---|---|
| Integration | CPU + RAM + Flash + I/O peripherals all on ONE chip | Just the CPU — needs external RAM, storage, I/O chips |
| Typical use | Embedded/dedicated control tasks (ESP32, Arduino, STM32) | General-purpose computing (PC/server CPUs) |
| Cost & power | Cheap, low power | More expensive, more powerful, more power-hungry |
| Example | ESP32-S3, ATmega328, STM32 | Intel Core i7, ARM Cortex-A in a phone SoC |

> **Exam trap**: ESP32-S3 is a **microcontroller**, not a microprocessor, even though it's powerful enough to feel "PC-like" — it integrates CPU, RAM (SRAM), flash controller, and dozens of peripherals on one chip.

## 5. Components of an embedded system
1. **Hardware**: MCU/processor, memory, sensors, actuators, power supply, communication interfaces
2. **Software**: firmware — the low-level code (often written in C) running directly on the hardware, sometimes on top of an RTOS (like FreeRTOS)
3. **Real-Time Operating System (optional but common)**: schedules multiple tasks with timing guarantees — this is where FreeRTOS fits in this course

## 6. The embedded system design lifecycle (typical exam list)
1. **Requirements specification** — define what the system must do, timing/power/cost constraints
2. **Hardware/software partitioning** — decide what's done in hardware (fast, fixed) vs software (flexible)
3. **Hardware design** — select MCU, sensors, peripherals, design the circuit/PCB
4. **Software/firmware development** — write and structure the code (often using an RTOS for complex systems)
5. **Integration and testing** — combine hardware + software, test against requirements, especially timing
6. **Deployment and maintenance** — release, then field updates (e.g. OTA — Over-The-Air updates)

## 7. Why embedded systems increasingly use an RTOS (bridges into this course's FreeRTOS content)
As embedded applications get more complex (multiple sensors, wireless comms, displays, all "at once"), a simple super-loop becomes unmanageable and can't give timing guarantees — this is exactly the motivation covered in the FreeRTOS notes ([[03 - FreeRTOS Fundamentals (Tasks and Scheduling)]]).

## 8. Quick self-test
1. Define an embedded system and give two examples not mentioned above.
2. Distinguish hard and soft real-time systems with an example of each.
3. Give three differences between a microcontroller and a microprocessor.
4. Is the ESP32-S3 a microcontroller or microprocessor, and why does that classification sometimes surprise people?
5. List the 6 typical stages of the embedded system design lifecycle.
6. Why does growing system complexity push embedded designers toward using an RTOS?
