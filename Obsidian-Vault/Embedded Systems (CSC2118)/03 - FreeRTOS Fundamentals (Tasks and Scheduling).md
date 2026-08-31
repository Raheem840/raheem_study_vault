---
topic: "Introduction to FreeRTOS — Part A"
title: FreeRTOS Fundamentals — Tasks, Scheduling, Priority, Context Switching
source: "Course slides (03_-_FreeRTOS.pdf) + FreeRTOS official docs + ESP-IDF guide"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material, freertos]
---

# FreeRTOS Part A — Tasks, Scheduling, and Context Switching

## 1. What problem does an RTOS actually solve?
A normal embedded program is often one giant `while(1)` "super loop" doing everything in sequence: read sensor → send data → update display → repeat. Problem: if any one step takes too long (or blocks waiting for something), *everything else waits too* — bad for time-sensitive systems (safety checks, real-time control).

A **Real-Time Operating System (RTOS)** guarantees **deterministic** behavior: tasks run predictably within a known time frame, instead of hoping the super loop gets around to them in time. FreeRTOS is the specific RTOS used on ESP32; it's already built into ESP-IDF, so creating tasks in `app_main()` gives you FreeRTOS "for free."

**Why FreeRTOS over the super loop, concretely:**
- **True multitasking** — read sensors, send data, and update a display all "simultaneously" (interleaved) without one blocking the others.
- **Better code organization** — each responsibility becomes its own independent task function instead of one tangled loop.
- **Precise timing** — `vTaskDelay()` / timers give accurate, non-blocking waits instead of busy-loops.
- **Responsiveness** — high-priority tasks (safety checks, interrupts) run immediately, even while lower-priority work is happening.
- **Scalability** — adding a feature = adding a new task with a priority, not rewriting the whole loop.

---

## 2. Tasks — the basic unit of work
A **task** is an independent C function that runs (conceptually) in parallel with other tasks. Each task has:
- A **priority** — determines how often/urgently it gets CPU time
- A **stack size** — how much memory it's allotted for local variables/call frames
- A **task function** — the actual code, written as an infinite loop (`while(1)`) that never returns

### Creating a task
```c
BaseType_t xTaskCreate(
    TaskFunction_t pvTaskCode,    // pointer to your task function
    const char * const pcName,   // name, for debugging only
    const uint32_t usStackDepth, // stack size in WORDS (not bytes!)
    void *pvParameters,          // optional data passed into the task
    UBaseType_t uxPriority,      // priority number
    TaskHandle_t *pvCreatedTask  // optional: handle to control this task later
);
```
Example — two independent LED blinkers:
```c
void blink_task_1(void *pvParameters){
    gpio_reset_pin(LED1);
    gpio_set_direction(LED1, GPIO_MODE_OUTPUT);
    while (1) {
        gpio_set_level(LED1, 1);
        vTaskDelay(pdMS_TO_TICKS(500));
        gpio_set_level(LED1, 0);
        vTaskDelay(pdMS_TO_TICKS(500));
    }
}
void app_main(void) {
    xTaskCreate(blink_task_1, "Blink1", 2048, NULL, 1, NULL);
}
```
Notice: `app_main()` itself is just the entry point that *creates* tasks and then returns — the actual work happens forever inside the tasks the scheduler runs.

---

## 3. Task states (memorize the 5 — very testable)
| State | Meaning |
|---|---|
| **Running** | Currently executing on the CPU (only one task per core at a time) |
| **Ready** | Able to run, just waiting for CPU time |
| **Blocked** | Waiting on a delay, queue, semaphore, or other event — NOT using CPU |
| **Suspended** | Explicitly paused (via `vTaskSuspend()`) until manually resumed |
| **Deleted** | Removed from memory entirely |

> **Exam trap**: "Blocked" and "Suspended" are often confused — Blocked is *temporary and automatic* (waiting for a specific event/timeout); Suspended is *indefinite and explicit* (only resumes when another task calls `vTaskResume()`).

---

## 4. The scheduler and priority
FreeRTOS uses a **preemptive** scheduler by default: the scheduler can interrupt ("preempt") a lower-priority running task the instant a higher-priority task becomes ready. The highest-priority **Ready** task is always the one that gets to **Run**.
- Priority is an integer; **higher number = higher importance** (no fixed upper limit; priority 0 is lowest).
- **Equal-priority tasks**: the scheduler shares CPU time between them via **round-robin time slicing** — each gets a "tick" interval before switching to the next, controlled by `configUSE_TIME_SLICING` and `configTICK_RATE_HZ`.

### Worked example (from your slides)
```c
xTaskCreate(Task1, "Task 1", 2048, NULL, 1, NULL);   // priority 1
xTaskCreate(Task2, "Task 2", 2048, NULL, 2, NULL);   // priority 2 — higher
```
Task2 (priority 2) always runs first; Task1 only gets CPU time when Task2 is blocked/delayed. This is **preemption by priority** — the core mental model to internalize.

### Changing priority at runtime
```c
vTaskPrioritySet(myTaskHandle, 3);   // bump a task's priority while running
```
The scheduler immediately re-evaluates and reallocates CPU time based on the new priority.

---

## 5. Delays — `vTaskDelay()` vs `vTaskDelayUntil()`
- **`vTaskDelay(ticks)`** — suspends the calling task for a number of ticks (1 tick ≈ 1ms by default), during which OTHER tasks are free to run. This is what makes RTOS delays "non-blocking" for the system overall (even though it blocks that one task).
- **`vTaskDelayUntil()`** — delays until a fixed, absolute point in time, regardless of how long the task's own code took to run. Prevents timing drift for periodic tasks (e.g. sampling a sensor exactly every 100ms even if the processing itself takes a variable amount of time each cycle).

---

## 6. Context switching — the mechanism underneath it all
**Context switching** = saving the current task's CPU state (registers, stack pointer, program counter) and restoring a different task's previously-saved state, so it resumes exactly where it left off.

**When it happens:**
- **Preemption** — a higher-priority task becomes ready
- **Time slicing** — equal-priority tasks rotate
- **Blocking** — the running task waits on a delay/event/semaphore
- **ISR yield** — an interrupt handler unblocks a higher-priority task

**The cost**: context switching is not free — each switch costs a few microseconds saving/restoring registers. FreeRTOS is optimized to keep this minimal, but extremely frequent switching (e.g. very short tick periods with many tasks) adds real overhead.

**On ESP32 specifically**: it's dual-core, so FreeRTOS runs a **separate scheduler per core** — you can even pin a task to a specific core with `xTaskCreatePinnedToCore()`.

### Tick rate tradeoff (testable table)
| Tick rate | Tick duration | Effect |
|---|---|---|
| 1000 Hz | 1 ms | High responsiveness, but more context switches → more overhead |
| 100 Hz | 10 ms | Balanced, general-purpose |
| 10 Hz | 100 ms | Low overhead, but sluggish timing — only for slow/low-power systems |

Set via `configTICK_RATE_HZ` in `FreeRTOSConfig.h`.

---

## 7. Preemptive vs Cooperative scheduling
| | Preemptive (FreeRTOS default) | Cooperative |
|---|---|---|
| Switching | Scheduler can interrupt a running task anytime | Task must voluntarily yield (`taskYield()`) or block |
| Responsiveness | High — critical tasks get the CPU immediately | Depends entirely on tasks yielding promptly |
| Overhead | More frequent switches, slightly more CPU cost | Less frequent, lower overhead |
| Risk | Low risk of one task blocking others | High — one long task can starve everything else |
| Best for | Real-time systems where timing matters | Simple, predictable systems with no strict timing needs |

---

## 8. Stack management
Every task has its **own stack** (local variables, return addresses, interrupt context). If a task uses more than its allocated stack, you get a **stack overflow** — a classic silent-corruption bug in embedded systems.

**Debug tool**: `uxTaskGetStackHighWaterMark()` returns the *minimum free stack space* the task has ever had during its lifetime — a large number means safe headroom; a small/near-zero number means you're close to overflow and should increase the stack size given at task creation.

---

## 9. Quick self-test
1. Why is a single `while(1)` super loop a poor fit for time-sensitive embedded systems?
2. List the 5 task states and define "Blocked" vs "Suspended" precisely.
3. In FreeRTOS priority, does a higher number mean more or less important?
4. If Task A has priority 2 and Task B has priority 1, which runs when both are ready? When does B ever get to run?
5. Contrast `vTaskDelay()` and `vTaskDelayUntil()` — which prevents timing drift, and why?
6. Name the 4 situations that trigger a context switch.
7. What does `uxTaskGetStackHighWaterMark()` tell you, and what does a small return value indicate?
8. Why does ESP32 (dual-core) run two separate FreeRTOS schedulers?
