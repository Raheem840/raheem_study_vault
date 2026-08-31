---
topic: "Outline Topic 5 — I/O and Sensor Interfacing and Programming"
source: "ESP-IDF Peripheral driver docs (free, docs.espressif.com) + standard embedded I/O concepts"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material, io, sensors]
---

# I/O and Sensor Interfacing and Programming

## 1. The three main ways a microcontroller talks to the outside world
1. **Digital I/O (GPIO)** — on/off signals: buttons, LEDs, relays
2. **Analog input (ADC)** — continuous voltage signals: potentiometers, analog temperature sensors
3. **Structured serial protocols** — I2C, SPI, UART — for "smart" sensors/devices that send structured digital data (covered in depth in [[09 - Serial Communication]])

## 2. Digital GPIO — input and output
```c
// Output (e.g. driving an LED or relay)
gpio_reset_pin(GPIO_NUM_2);
gpio_set_direction(GPIO_NUM_2, GPIO_MODE_OUTPUT);
gpio_set_level(GPIO_NUM_2, 1);

// Input (e.g. reading a push-button)
gpio_set_direction(GPIO_NUM_4, GPIO_MODE_INPUT);
int state = gpio_get_level(GPIO_NUM_4);
```
**Pull-up/pull-down resistors**: a digital input pin left unconnected ("floating") can read random noise as 0 or 1. A pull-up resistor holds the pin HIGH by default (button press pulls it LOW); a pull-down does the opposite. ESP-IDF lets you enable the chip's **internal** pull resistors via `gpio_set_pull_mode()` instead of needing an external resistor for simple cases.

## 3. Interrupts (ISR) — reacting to a GPIO change immediately
Instead of continuously polling a pin in a loop (wasting CPU), you can register an **interrupt service routine (ISR)** that fires automatically on a pin's rising/falling edge:
```c
gpio_install_isr_service(0);
gpio_isr_handler_add(GPIO_NUM_4, my_isr_handler, NULL);
```
**Critical rule**: ISR code must be extremely short and fast — no `printf()`, no blocking calls. The standard pattern is: ISR gives a semaphore/task notification (see [[04 - FreeRTOS Inter-Task Communication]] and [[05 - FreeRTOS Timers and Task Notifications]]) to wake a normal task, which does the actual (slower) processing outside the ISR context. Use `xSemaphoreGiveFromISR()` / `xTaskNotifyGiveFromISR()` — the ISR-safe variants — not the regular versions, inside an ISR.

## 4. Analog input — ADC in practice
```c
adc_oneshot_unit_handle_t adc1_handle;
// ... configure unit and channel ...
int raw_value;
adc_oneshot_read(adc1_handle, ADC_CHANNEL_6, &raw_value);
```
The raw value (e.g. 0–4095 for 12-bit resolution) is proportional to input voltage relative to the reference range — convert to actual voltage/physical units using the sensor's datasheet formula, e.g. for a simple linear temperature sensor: `voltage = raw * Vref / 4095`, then `temp_C = (voltage - offset) / scale`.

## 5. PWM — Pulse Width Modulation (for analog-like output)
Microcontrollers can't output a true analog voltage directly (only HIGH/LOW), so **PWM** simulates variable "strength" by rapidly switching a digital pin on/off with a controlled **duty cycle** (% of time HIGH). Higher duty cycle ≈ more average power delivered — used for LED dimming, motor speed control, generating audio tones.
- ESP-IDF's PWM peripheral is called **LEDC** (LED Controller, despite the name, used generally for PWM).
```c
ledc_timer_config(&timer_conf);      // sets frequency & resolution
ledc_channel_config(&channel_conf);  // binds a GPIO pin + duty cycle
ledc_set_duty(LEDC_LOW_SPEED_MODE, LEDC_CHANNEL_0, 512); // ~50% at 10-bit res
ledc_update_duty(LEDC_LOW_SPEED_MODE, LEDC_CHANNEL_0);
```

## 6. Common sensor interfacing patterns (exam-relevant, general knowledge)
| Sensor type | Interface used | Typical driving pattern |
|---|---|---|
| Push button, PIR motion sensor | Digital GPIO input (+ debounce or interrupt) | Poll or ISR |
| Potentiometer, analog temp/light sensor (LDR) | ADC | Periodic read in a task, e.g. via a timer-triggered notification |
| DHT11/DHT22 (temp+humidity) | Single-wire digital protocol (precise timing, often bit-banged) | Careful timed pulses; sensitive to task delays, so often done with interrupts disabled briefly |
| BMP280/BME280 (pressure/temp/humidity), most modern sensors | I2C | Read via `i2c_master_*` driver calls |
| Ultrasonic distance sensor (HC-SR04) | Digital trigger pulse out + digital echo pulse in, timed | Measure pulse width to compute distance |

## 7. Debouncing — a classic practical gotcha
A mechanical button "bounces" (rapidly toggles) for a few milliseconds when pressed/released, which can register as multiple presses. Fix: either a short software delay/recheck after detecting a change (**debounce delay**, e.g. 20-50ms), or a hardware RC filter, or a debounce library — this is a very common practical exam/lab question.

## 8. Quick self-test
1. Name the three main categories of I/O a microcontroller uses to interact with the outside world.
2. Why does a floating digital input pin misbehave, and what fixes it?
3. Why must ISR code be short, and what's the correct pattern for doing real work in response to an interrupt?
4. What does PWM duty cycle control, and name ESP-IDF's PWM peripheral name.
5. Convert a raw 12-bit ADC reading of 2048 to a voltage, assuming a 3.3V reference.
6. What is switch/button "bounce," and name two ways to handle it.
7. Which sensor type from the table above would you choose to measure humidity, and what interface does it typically use?
