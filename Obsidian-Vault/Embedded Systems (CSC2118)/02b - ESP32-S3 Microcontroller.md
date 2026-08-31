---
topic: "Outline Topic 2 — ESP32-S3 Microcontroller"
source: "Espressif ESP32-S3 Technical Reference Manual & Datasheet (free, docs.espressif.com)"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material, esp32]
---

# ESP32-S3 Microcontroller

## 1. What it is
The **ESP32-S3** is a low-cost, low-power system-on-chip (SoC) microcontroller from Espressif, with integrated Wi-Fi and Bluetooth Low Energy (BLE), designed for IoT and edge-AI applications. It's the successor to the original ESP32, with more GPIO, AI-acceleration instructions, and native USB.

## 2. Core architecture (know these numbers)
| Feature | ESP32-S3 spec |
|---|---|
| CPU | Dual-core **Xtensa LX7** 32-bit processor |
| Clock speed | Up to 240 MHz |
| SRAM | 512 KB on-chip |
| Additional memory | Optional external PSRAM (Pseudo-static RAM) for large buffers (e.g. camera frames) |
| Flash | External SPI flash (typically 4–16 MB on dev boards) for program storage |
| Wireless | Wi-Fi 802.11 b/g/n (2.4 GHz) + Bluetooth 5 (LE) |
| GPIO | Up to 45 programmable GPIO pins |
| Special feature | Vector instructions for AI/ML acceleration (e.g. speeding up neural network inference at the edge) |
| USB | Native USB OTG (can act as a USB device directly — no separate USB-to-serial chip needed on some boards) |

> **Exam trap**: SRAM (fast, on-chip, used for running variables/stack) is different from Flash (slower, off-chip typically, stores the compiled program persistently) — don't conflate the two when asked about "where does the program live vs where does it run."

## 3. Why dual-core matters for FreeRTOS (ties directly to Part A notes)
The two cores let FreeRTOS run genuinely parallel task execution (not just time-sliced on one core). By default, Core 0 often handles Wi-Fi/BT protocol stack tasks, while Core 1 is free for your application tasks — but you can pin any task to either core explicitly with `xTaskCreatePinnedToCore()`.

## 4. GPIO (General Purpose Input/Output)
Each GPIO pin can be configured as digital input or output, and many support special "alternate functions" (I2C, SPI, UART, ADC, PWM) — pin capability varies, not every pin does everything. Basic configuration in ESP-IDF:
```c
gpio_reset_pin(GPIO_NUM_2);
gpio_set_direction(GPIO_NUM_2, GPIO_MODE_OUTPUT);
gpio_set_level(GPIO_NUM_2, 1);   // set HIGH
```

## 5. ADC (Analog-to-Digital Converter)
Converts a continuous analog voltage (e.g. from a potentiometer or analog sensor) into a discrete digital value the CPU can process. ESP32-S3 has multiple ADC channels (grouped into ADC1/ADC2 units), with configurable resolution (commonly up to 12-bit, giving values 0–4095) and attenuation settings (to extend the measurable voltage range beyond the default ~1.1V reference).

## 6. Power modes (testable list)
| Mode | Description | Typical use |
|---|---|---|
| **Active** | Full power, CPU running normally | Normal operation |
| **Modem-sleep** | CPU runs, but Wi-Fi/BT radio powered down between beacon intervals | Wi-Fi-connected but idle |
| **Light-sleep** | CPU paused, RAM retained, wakes quickly on an interrupt/timer | Short idle periods, need fast wake |
| **Deep-sleep** | Only the RTC (real-time clock) domain stays powered; most of the chip (including main RAM) is off; wakes via reset/reboot from RTC or external pin | Battery-powered devices sleeping for long periods |

> **Exam trap**: Deep-sleep wake is effectively a **reboot** (`app_main()` runs again) — unlike light-sleep, which resumes exactly where the code left off. Any state you need to survive deep-sleep must be stored in RTC memory (`RTC_DATA_ATTR`) or non-volatile storage.

## 7. Why the ESP32-S3 fits an FreeRTOS-based curriculum
Its dual-core, always-on Wi-Fi/BT stack, and multiple concurrent I/O needs (sensors + wireless + display, etc.) make a super-loop impractical — this is the direct hardware justification for building this whole course around FreeRTOS.

## 8. Quick self-test
1. Name the CPU architecture, core count, and max clock speed of the ESP32-S3.
2. Distinguish SRAM from Flash on this chip — which holds the running program's variables, and which stores the compiled firmware?
3. What's the practical benefit of having two CPU cores for a FreeRTOS application?
4. What does `xTaskCreatePinnedToCore()` let you do that plain `xTaskCreate()` doesn't guarantee?
5. List the 4 ESP32 power modes from highest to lowest power consumption, and state one property of deep-sleep that differs fundamentally from light-sleep.
6. What ADC resolution does ESP32-S3 commonly support, and what range of digital values does that give?
