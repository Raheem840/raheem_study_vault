---
topic: "Prerequisite — C for Embedded Systems"
source: "Foundational — needed to understand every FreeRTOS/ESP-IDF example in this course"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material, c-programming]
---

# C Programming Foundations for Embedded Systems

## 1. Why this note exists
Every FreeRTOS/ESP-IDF code example uses C idioms that university "intro to programming" courses often skip. If you don't have these cold, the FreeRTOS notes will feel like magic syntax instead of logic you understand. This is the "decoder ring" for every line of code you'll see this semester.

---

## 2. Pointers — the single most important embedded C concept
A pointer is a variable that stores a **memory address** rather than a value.
```c
int x = 5;
int *p = &x;      // p now holds the address of x
*p = 10;          // dereference p — this changes x to 10
```
Why embedded C leans on pointers constantly:
- **Passing data to tasks**: `void myTask(void *pvParameters)` — every FreeRTOS task function receives a `void*` so it can accept ANY type of data, cast to what you actually need inside.
- **Passing "by reference"**: `xQueueReceive(queue, &receivedValue, ...)` passes the *address* of `receivedValue` so the function can write into it directly, instead of returning a copy.
- **Hardware registers**: microcontroller peripherals (GPIO, UART, timers) are literally memory addresses — a pointer to a specific address lets your program directly control hardware.

## 3. `void*` — the "generic" pointer
`void *pvParameters` in every task function means "a pointer to something, type unspecified." Inside the task, you cast it to the real type before using it:
```c
void myTask(void *pvParameters) {
    int *pParam = (int *) pvParameters;   // cast back to the real type
    printf("Value: %d\n", *pParam);
}
```
This is how FreeRTOS lets one task-creation function (`xTaskCreate`) work for tasks that need completely different kinds of parameters.

## 4. `struct` — grouping related data
```c
typedef struct {
    float temperature;
    float humidity;
} SensorData_t;
```
Used constantly to pass a bundle of related values through a **queue** in one send (rather than needing separate queues for temperature and humidity).

## 5. Function pointers — how callbacks work
A function pointer stores the *address of a function*, so you can pass "which function to call later" as a value:
```c
void myCallback(void) { printf("Timer fired!\n"); }
void (*callbackPtr)(void) = myCallback;   // callbackPtr now "points to" myCallback
callbackPtr();                             // calls myCallback()
```
This is exactly what's happening with `xTimerCreate(..., myTimerCallback)` — you're handing FreeRTOS the *address* of your callback function, which it calls automatically when the timer expires. `TaskFunction_t pvTaskCode` in `xTaskCreate()` is the same idea for tasks.

## 6. `volatile` — telling the compiler "this can change unexpectedly"
```c
volatile int flag = 0;
```
Normally the compiler optimizes by caching a variable's value in a CPU register. `volatile` tells it: "don't assume — always re-read this from memory," because the value might be changed by an interrupt (ISR) or another task at any time. **Forgetting `volatile` on a variable shared with an ISR is one of the most common embedded bugs** — the compiler may optimize away the re-check, so your code never "sees" the update.

## 7. `static` — two very different meanings depending on context
- **Inside a function**: the variable keeps its value between calls (initialized once, persists).
- **At file scope**: the variable/function is only visible within that `.c` file (not exported) — used to keep helper functions/globals private to a module.

## 8. Bitwise operators — controlling hardware registers bit-by-bit
Hardware registers (e.g. GPIO configuration) are often a single 32-bit number where each **bit** controls a different setting. You manipulate individual bits without disturbing the others:
| Operator | Meaning | Typical use |
|---|---|---|
| `&` (AND) | Check/clear specific bits | `if (reg & (1<<3))` — check if bit 3 is set |
| `\|` (OR) | Set specific bits | `reg \|= (1<<3);` — set bit 3 to 1 |
| `^` (XOR) | Toggle specific bits | `reg ^= (1<<3);` — flip bit 3 |
| `~` (NOT) | Invert all bits | `reg &= ~(1<<3);` — clear bit 3 (combined with AND) |
| `<<` `>>` | Shift bits left/right | `(1<<3)` = binary `00001000` (bit 3 set) |

## 9. `#include` and header files
`#include "freertos/FreeRTOS.h"` pulls in **declarations** (function prototypes, types, macros) from that header file so the compiler knows these functions/types exist before you use them; the actual code (definitions) is compiled/linked in separately from the ESP-IDF library. Angle brackets `<>` search system/library paths; quotes `""` search your project's local folders first.

## 10. Macros and constants you'll see everywhere
- `pdMS_TO_TICKS(1000)` — a macro that converts milliseconds into the RTOS's internal "tick" units (since the OS scheduler thinks in ticks, not ms).
- `pdTRUE` / `pdFALSE` — FreeRTOS's own boolean constants (used instead of plain `1`/`0` for readability and portability).
- `NULL` — a pointer that points to "nothing"; always check handles against `NULL` after creation (`if (myTimer == NULL)`) because embedded systems have very limited memory and creation can fail silently otherwise.

## 11. Quick self-test
1. What does `int *p = &x;` do, in plain English?
2. Why does every FreeRTOS task function take a `void *pvParameters` argument instead of a specific type?
3. What bug does `volatile` prevent, and why does it matter for variables touched by an ISR?
4. Write the bitwise expression to set bit 5 of a register, and the one to clear it.
5. What's the difference between a `static` variable declared inside a function vs. at file scope?
6. What is a function pointer, and where have you already seen one used in the FreeRTOS slides?
