---
topic: "Introduction to FreeRTOS — Part C"
title: FreeRTOS Software Timers and Task Notifications
source: "Course slides (03_-_FreeRTOS.pdf) + FreeRTOS official docs"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material, freertos]
---

# FreeRTOS Part C — Software Timers & Task Notifications

## 1. Why not just use `vTaskDelay()` for everything periodic?
`vTaskDelay()` works, but it **blocks the calling task** for the entire wait — that task can't do anything else while waiting. A **software timer** runs entirely in the background: you register a callback function, and FreeRTOS calls it automatically when the time expires, without dedicating a whole task (and its stack) just to waiting.

## 2. Hardware timers vs software timers
| | Hardware Timer | Software Timer |
|---|---|---|
| Runs on | The microcontroller's internal hardware counter | FreeRTOS kernel (via the "Timer Service Task") |
| Accuracy | Very high | Depends on the system tick rate |
| Triggered by | Interrupt | Handled in software, no dedicated interrupt |
| Callback context | Runs in an ISR | Runs in normal task context |
| Best for | Precise, low-latency timing (PWM, exact sensor sampling) | General periodic tasks where ms-level precision is fine |

## 3. The Timer Service Task ("Timer Daemon")
All software timers are actually managed by one special background task that FreeRTOS creates automatically. It handles creating/deleting timers, starting/stopping them, and executing their callbacks — so your timer callbacks run in normal task context, not inside an interrupt.

## 4. Creating and using a software timer
```c
TimerHandle_t myTimer;

void myTimerCallback(TimerHandle_t xTimer) {
    printf("Timer expired! Performing action...\n");
}

void app_main(void) {
    myTimer = xTimerCreate(
        "MyTimer",              // name (debugging only)
        pdMS_TO_TICKS(1000),    // period: 1000ms
        pdTRUE,                 // auto-reload? pdTRUE = repeats, pdFALSE = one-shot
        0,                      // timer ID (optional, can identify which timer fired)
        myTimerCallback         // callback function to run on expiry
    );
    if (myTimer == NULL) { printf("Timer creation failed!\n"); }
    else { xTimerStart(myTimer, 0); }   // timers do NOT start automatically!
}
```
**One-shot vs Auto-reload**:
- `pdFALSE` → **one-shot** — fires once, then you must call `xTimerStart()` again manually if you want it to repeat.
- `pdTRUE` → **auto-reload** — restarts itself automatically after every expiration (e.g. periodic LED blink or sensor poll).

### Other timer control functions
| Function | Effect |
|---|---|
| `xTimerStart()` | Starts a stopped timer |
| `xTimerStop()` | Stops a running timer |
| `xTimerReset()` | Restarts the timer's countdown from zero |
| `xTimerChangePeriod()` | Changes the period while running |
| `xTimerDelete()` | Deletes the timer entirely |

> **Exam trap**: creating a timer with `xTimerCreate()` does **not** start it — you must explicitly call `xTimerStart()`, otherwise it just sits idle.

---

## 5. Task Notifications — the lightest-weight signaling tool
A **task notification** lets one task (or an ISR) send a small 32-bit value directly to another specific task, without creating a separate queue or semaphore object. It's essentially "a semaphore/queue built into every task for free."

```c
// Sender:
xTaskNotifyGive(Task2Handle);          // simplest form — like "giving" a binary semaphore
// or, from an ISR:
xTaskNotifyGiveFromISR(taskHandle, NULL);
// or with a custom 32-bit value:
xTaskNotify(taskHandle, 0x01, eSetValueWithOverwrite);

// Receiver:
ulTaskNotifyTake(pdTRUE, portMAX_DELAY);   // pdTRUE = clear the count after taking
// or, to also retrieve the value:
xTaskNotifyWait(0x00, 0x00, &notificationValue, portMAX_DELAY);
```

### Why prefer notifications over a queue/semaphore when possible
| Feature | Queue | Semaphore | Task Notification |
|---|---|---|---|
| Memory usage | High (needs a buffer) | Medium | **Very low** (no separate object — built into the task's own TCB) |
| Speed | Slower | Moderate | **Fastest** |
| Data | Any type (struct, bytes...) | Binary/counting only | A single 32-bit integer |
| Use case | Passing structured data | Synchronization | Signaling + simple counters |

Trade-off: notifications are the fastest and lightest, but they can only target **one specific task** with **one integer value** — for structured multi-field data (e.g. temperature + humidity together) you still need a queue.

---

## 6. Combining a timer with a notification (a common real pattern)
Instead of dedicating a whole task just to waiting on a delay, a timer's callback can notify a real worker task the moment it's time to act:
```c
void myTimerCallback(TimerHandle_t xTimer) {
    xTaskNotifyGive(workerTaskHandle);   // wake the worker task up
}
// workerTask blocks efficiently until notified:
void workerTask(void *pv) {
    while (1) {
        ulTaskNotifyTake(pdTRUE, portMAX_DELAY);
        // do the actual work — e.g. read a sensor, send over UART
    }
}
```
This pattern (timer → notify → worker task acts) is more efficient than a worker task doing its own `vTaskDelay()` loop, especially when several independent timers need to trigger different actions on the same or different tasks.

---

## 7. Quick self-test
1. Why use a software timer instead of just calling `vTaskDelay()` inside a task?
2. Contrast hardware and software timers on accuracy and callback context.
3. What task actually executes every software timer's callback function?
4. What's the difference between a one-shot and an auto-reload timer, and which xTimerCreate parameter controls it?
5. Does `xTimerCreate()` start the timer? What do you need to call afterward?
6. What is a task notification, and why is it faster/lighter than a queue or semaphore?
7. What's the main limitation of task notifications compared to a queue?
8. Describe the "timer triggers a notification, which wakes a worker task" pattern and why it's more efficient than a busy `vTaskDelay()` loop in the worker.
