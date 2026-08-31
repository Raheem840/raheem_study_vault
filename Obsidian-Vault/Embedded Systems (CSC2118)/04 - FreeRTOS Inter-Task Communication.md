---
topic: "Introduction to FreeRTOS — Part B"
title: FreeRTOS Inter-Task Communication — Queues, Semaphores, Mutexes
source: "Course slides (03_-_FreeRTOS.pdf) + FreeRTOS official docs"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material, freertos]
---

# FreeRTOS Part B — Queues, Semaphores, and Mutexes

## 1. Why tasks need to talk to each other
Tasks are independent, but real systems need them to cooperate: a sensor task collects data, a display task shows it, a logger task saves it. Without careful coordination, sharing data between tasks causes:
- **Data corruption** — one task writes while another reads simultaneously
- **Race conditions** — the result depends on unpredictable execution order/timing
- **Deadlocks** — two tasks each wait forever for a resource the other holds
- **Unpredictable crashes** — from unsafe access to shared memory/peripherals

FreeRTOS gives you four coordination tools: **Queues** (pass data), **Semaphores** (signal events), **Mutexes** (protect shared resources), **Event Groups** (multi-event coordination, not covered here).

---

## 2. Queues — passing data safely between tasks
A **queue** is a FIFO (First-In-First-Out) buffer for transferring data between tasks, without either task worrying about timing conflicts.

```c
QueueHandle_t testQueue = xQueueCreate(5, sizeof(int));   // holds up to 5 ints
if (testQueue == NULL) { printf("Failed to create queue\n"); }
```
`xQueueCreate(length, item_size)` — **length** = how many items it can hold, **item_size** = bytes per item.

**Sending and receiving:**
```c
xQueueSend(testQueue, &dataToSend, portMAX_DELAY);       // producer
xQueueReceive(testQueue, &receivedData, portMAX_DELAY);  // consumer
```
The third argument is how long (in ticks) the task should **block** waiting if the queue is full (send) or empty (receive); `portMAX_DELAY` means wait forever.

**What queues guarantee:**
- Data arrives in the order it was sent
- No two tasks access the same queue slot simultaneously
- A receiving task can block until new data actually arrives (efficient — not polling/busy-waiting)

**Worked pattern** (Sender/Receiver from your slides): a `SenderTask` increments a value every second and pushes it via `xQueueSend`; a `ReceiverTask` blocks on `xQueueReceive` and prints whatever arrives. Both run independently, fully decoupled, communicating only through the queue.

---

## 3. Semaphores — signaling between tasks (or ISR → task)
A semaphore is a signaling mechanism, not a data channel.

### Binary semaphore
Only two states: **taken (0)** or **available (1)**. Classic use: one task (or an interrupt) "gives" the semaphore to signal an event; another task "takes" it to proceed once that event occurs.
```c
SemaphoreHandle_t dataReadySemaphore = xSemaphoreCreateBinary();
// Producer side (e.g. ADCTask):
xSemaphoreGive(dataReadySemaphore);      // signal "new data is ready"
// Consumer side (e.g. UARTTask):
if (xSemaphoreTake(dataReadySemaphore, portMAX_DELAY)) {
    // proceed — new data is confirmed ready
}
```
Common real use: an ISR "gives" a semaphore to unblock a waiting task the moment a hardware event occurs (e.g. data-ready interrupt), without the task needing to poll.

### Counting semaphore
Works like a counter tracking how many instances of a resource are available (e.g. a pool of N buffers). Each `xSemaphoreGive()` increments the count; each `xSemaphoreTake()` decrements it — useful when more than one task can hold "a" resource simultaneously, up to some limit.

> **Exam trap**: Binary semaphore = one signal, one consumer action. Counting semaphore = a running tally against a pool of interchangeable resources. Don't describe a binary semaphore as "counting from 0 up" — it only ever has two states.

---

## 4. Mutexes — protecting shared resources
A **mutex** (MUTual EXclusion) works like a lock: once a task takes it, every other task trying to take it must wait until it's released.

```c
SemaphoreHandle_t sharedMutex = xSemaphoreCreateMutex();
// Any task wanting the shared resource:
if (xSemaphoreTake(sharedMutex, portMAX_DELAY)) {
    // ... use the shared UART/I2C/SPI bus or variable safely ...
    xSemaphoreGive(sharedMutex);
}
```
This prevents two tasks from writing to, say, the same I²C bus simultaneously and corrupting the transmission.

### Semaphore vs Mutex — THE classic exam comparison table
| Feature | Semaphore | Mutex |
|---|---|---|
| Purpose | Synchronization / signaling | Protecting a shared resource |
| Ownership | None — any task can give or take | Owned by the specific task that took it |
| Priority inheritance | Not supported | **Supported** |
| Typical use | Task-to-task or ISR-to-task signaling | Locking a bus/peripheral/shared variable |

### Priority inheritance — why mutexes specifically need it
Scenario (straight from your slides): LowPriorityTask takes a mutex and holds it doing a long operation. MediumPriorityTask keeps running unrelated background work at a higher priority than Low. HighPriorityTask then also wants the mutex and must wait for Low to release it.

**Problem without priority inheritance ("priority inversion")**: Medium (higher priority than Low, but not needing the mutex) keeps preempting Low, so Low never gets to finish and release the mutex — meaning High effectively waits on Medium, even though High "outranks" Medium. This is a bug where a high-priority task is indirectly blocked by a low-priority one via a chain of unrelated tasks.

**The fix — priority inheritance**: while Low is holding a mutex that a higher-priority task is waiting for, FreeRTOS **temporarily boosts Low's priority** to match the waiting task's priority, so Medium can no longer preempt it. Low finishes and releases the mutex faster, then drops back to its normal (low) priority. This is exactly why mutexes support priority inheritance and plain semaphores don't — a semaphore has no "owner" for FreeRTOS to boost.

---

## 5. Quick self-test
1. Name the four coordination tools FreeRTOS provides, and what each is fundamentally for (data vs. signal vs. lock vs. multi-event).
2. What do the two parameters to `xQueueCreate()` mean?
3. What does passing `portMAX_DELAY` as the wait time to `xQueueSend`/`xQueueReceive` mean?
4. Distinguish a binary semaphore from a counting semaphore with an example use case for each.
5. What does "ownership" mean for a mutex, and why does a semaphore not have it?
6. Explain priority inversion in your own words, using the Low/Medium/High task example.
7. How does priority inheritance solve priority inversion?
8. Why is a mutex the right tool (not a plain semaphore) for protecting a shared I²C bus?
