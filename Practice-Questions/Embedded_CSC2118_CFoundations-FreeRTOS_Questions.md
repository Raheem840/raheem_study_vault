# CSC 2118 — Practice Questions: C Foundations + FreeRTOS (Tasks, Communication, Timers)

Closed-book. Cover the answer key until you've committed to an answer.

## C Foundations
1. Write a task function signature that receives a pointer to a `SensorData_t` struct, and show how you'd cast `pvParameters` back to that type inside the function.
2. A shared flag `int dataReady = 0;` is set to 1 inside an ISR and checked in a `while(!dataReady)` loop in a task. The compiler optimizes the loop into an infinite loop that never re-reads memory. What single keyword fixes this, and why?
3. Write the bitwise line to check whether bit 2 of a register `status` is set, without modifying `status`.

## FreeRTOS Fundamentals (Tasks & Scheduling)
4. Two tasks: `SensorTask` (priority 3) and `LoggerTask` (priority 1). Both are Ready. Which runs, and under what condition does LoggerTask ever get CPU time?
5. Explain why `vTaskDelay()` inside a task doesn't freeze the whole system, even though it blocks that one task.
6. A task's `uxTaskGetStackHighWaterMark()` steadily decreases toward 0 over the program's runtime. What does this indicate, and what should you do?
7. Give one advantage and one disadvantage of raising `configTICK_RATE_HZ` from 100Hz to 1000Hz.

## FreeRTOS Communication (Queues/Semaphores/Mutexes)
8. Design a mini system: a `TempSensorTask` reads a temperature every second and a `DisplayTask` shows the latest value. Which coordination tool fits best, and why (name the specific FreeRTOS calls you'd use)?
9. Two tasks both need exclusive access to the same UART port for a few hundred milliseconds each. Which tool fits — a binary semaphore or a mutex — and specifically why does ownership matter here?
10. Explain, step by step, how priority inversion could occur with a `LowPriorityTask` holding a mutex, a `MediumPriorityTask` unrelated to it, and a `HighPriorityTask` waiting for the mutex — and how FreeRTOS's mutex implementation prevents it.

## FreeRTOS Timers & Notifications
11. You need an LED to blink every 250ms indefinitely, without dedicating a whole task to a `vTaskDelay()` loop. Design it using a software timer — give the `xTimerCreate()` call with correct auto-reload setting.
12. Why might a task notification be a better fit than a queue for "wake this exact task up when a button is pressed", but a poor fit for "send this task the latest 3-field sensor struct"?
13. What's wrong with this code, and what's missing?
```c
TimerHandle_t t = xTimerCreate("T", pdMS_TO_TICKS(500), pdTRUE, 0, myCallback);
// ... nothing else ...
```

---

## ANSWER KEY

**1.**
```c
void sensorTask(void *pvParameters) {
    SensorData_t *pData = (SensorData_t *) pvParameters;
    printf("Temp: %.1f\n", pData->temperature);
}
```

**2.** `volatile`. Without it, the compiler assumes `dataReady` can't change during the loop (since nothing inside the loop's visible code modifies it) and may cache its value in a register or optimize the check away entirely — `volatile` forces a fresh memory read every time, so the task correctly notices when the ISR sets it.

**3.** `if (status & (1 << 2)) { /* bit 2 is set */ }` — using `&` reads the bit without altering `status` (no `=` assignment).

**4.** SensorTask (priority 3) runs — the scheduler always runs the highest-priority Ready task. LoggerTask only gets CPU time when SensorTask is Blocked (e.g. in a `vTaskDelay()` or waiting on a queue/semaphore) or Suspended.

**5.** `vTaskDelay()` moves the calling task into the **Blocked** state for the delay duration — it's removed from contention for the CPU, but the scheduler is completely free to run any other Ready task during that time. Only that one task "waits"; the system as a whole keeps working.

**6.** It indicates the task is getting closer to a stack overflow over time (possibly due to growing local data, recursion, or a leak-like pattern in stack usage). Fix: increase the stack size given in `xTaskCreate()`'s `usStackDepth` parameter, and/or review the task function for unnecessarily large local variables or unbounded recursion.

**7.** Advantage: finer timing resolution (1ms vs 10ms), meaning `vTaskDelay()`/timers are more precise and the system is more responsive to time-sensitive events. Disadvantage: more frequent tick interrupts mean more frequent scheduler evaluation and potential context switches, increasing CPU overhead purely from timekeeping.

**8.** A **queue** fits best: `xQueueCreate(N, sizeof(float))`, `TempSensorTask` calls `xQueueSend(tempQueue, &reading, portMAX_DELAY)` every second, `DisplayTask` calls `xQueueReceive(tempQueue, &latest, portMAX_DELAY)` and blocks efficiently until a new reading arrives. A queue fits because actual data (not just a signal) needs to move between the two tasks, in order, safely.

**9.** A **mutex** — because this is exclusive access to a shared resource with a clear "owner" while it's in use (one task holds the UART, uses it, then releases it). Ownership matters because FreeRTOS can apply priority inheritance to the specific owning task if a higher-priority task is waiting — a plain semaphore has no owner to boost, so it can't offer this protection against priority inversion.

**10.** Low takes the mutex to use a shared resource and starts a long operation. Medium, at a higher priority than Low but not needing the mutex, becomes ready and preempts Low repeatedly for unrelated background work — Low keeps getting suspended before it can finish and release the mutex. Meanwhile High, needing the mutex, is blocked waiting for Low to release it — so effectively Medium (lowest-priority task in this triangle that actually gets CPU time) is indirectly delaying High, inverting the intended priority order. FreeRTOS's mutex prevents this via **priority inheritance**: the instant a higher-priority task blocks waiting on a mutex held by Low, Low's priority is temporarily raised to match, so Medium can no longer preempt it — Low finishes and releases the mutex promptly, then drops back to its original priority.

**11.**
```c
TimerHandle_t blinkTimer = xTimerCreate("BlinkTimer", pdMS_TO_TICKS(250), pdTRUE, 0, blinkCallback);
if (blinkTimer != NULL) xTimerStart(blinkTimer, 0);
```
`pdTRUE` for auto-reload is essential — a one-shot timer would only blink once.

**12.** Task notifications target exactly one task with one 32-bit value — perfect for a simple "wake up now" signal like a button press (a binary event, one receiver). They're a poor fit for a 3-field sensor struct because a notification can't carry structured multi-field data — you'd need a queue (which can hold a whole struct per item) instead.

**13.** The timer is created but `xTimerStart()` is never called, so it never actually begins running — `xTimerCreate()` only allocates and configures the timer, it does not start it. Also missing: a NULL check on `t` to confirm creation succeeded before use.
