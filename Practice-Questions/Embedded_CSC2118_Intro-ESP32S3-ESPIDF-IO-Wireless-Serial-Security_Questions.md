# CSC 2118 — Practice Questions: Intro/ESP32-S3/ESP-IDF/IO/Wireless/Serial/Security

Closed-book. Cover the answer key until you've committed to an answer.

## Intro to Embedded Systems & ESP32-S3
1. Classify each as hard or soft real-time and justify: (a) a car's airbag deployment, (b) a smart thermostat's display refresh.
2. Explain why the ESP32-S3 is a microcontroller rather than a microprocessor, referencing what's integrated on the chip.
3. Your device must sleep for 8 hours on battery power between readings, but needs to remember a counter value across each sleep cycle. Which power mode fits, and how do you preserve the counter?

## ESP-IDF Environment
4. Write the full command sequence (as you'd type it) to set the target chip, build, flash, and monitor a new ESP-IDF project on port `/dev/ttyUSB0`.
5. A student's `idf.py build` fails with a chip-mismatch error. What single missing step most likely caused this?

## I/O and Sensor Interfacing
6. You need to detect a button press without wasting CPU cycles polling it every loop iteration, and without blocking other tasks. Design the approach (which mechanism, what's ISR-safe about it).
7. Given a 12-bit ADC reading of 3000 with a 3.3V reference, compute the corresponding voltage.
8. Explain why `printf()` inside a GPIO interrupt handler is dangerous, and what the correct alternative pattern is.

## Wireless Communication
9. Design the wireless architecture for a battery-powered soil moisture sensor that reports every 6 hours to a phone app when the user is nearby, with no internet needed. Which technology, and why?
10. Why is Wi-Fi connection status checked via an event handler rather than checked as a return value immediately after `esp_wifi_connect()`?

## Serial Communication
11. You need to connect 8 different I2C sensors and 2 SPI devices to one ESP32-S3. Compare the GPIO pin cost of each approach for these numbers of devices.
12. A UART link works fine at short range but starts corrupting data when a much longer cable is used. Baud rate and wiring are both correct at both ends. What UART-specific factor is the most likely underlying issue at very long line lengths/high baud rates, based on what you know about the protocol's electrical requirements?

## Security
13. Design a secure OTA update flow for a fielded IoT product from these primitives: TLS, Secure Boot, firmware signing. Describe the sequence of checks and why each is necessary.

---

## ANSWER KEY

**1.** (a) Hard real-time — a late airbag deployment is a failure regardless of correctness, since the deadline itself is safety-critical. (b) Soft real-time — a slightly delayed display refresh degrades user experience but isn't dangerous or catastrophic.

**2.** The ESP32-S3 integrates the CPU (dual-core Xtensa LX7), SRAM, a flash controller, and dozens of peripherals (GPIO, ADC, UART, I2C, SPI, Wi-Fi/BLE radios) all on one chip — this single-chip integration for a dedicated embedded task is exactly the microcontroller definition, versus a microprocessor which is just the CPU core needing external RAM/storage/IO chips.

**3.** Deep-sleep fits (lowest power, appropriate for a long fixed interval with no need to respond to external events during the sleep). Since main RAM is powered off during deep-sleep, the counter must be stored either in RTC memory (declared with `RTC_DATA_ATTR`, which survives deep-sleep specifically) or in NVS (non-volatile flash storage) if it needs to survive power loss entirely, not just deep-sleep.

**4.**
```
idf.py set-target esp32s3
idf.py build
idf.py -p /dev/ttyUSB0 flash monitor
```

**5.** Most likely: forgetting to run `idf.py set-target esp32s3` before the first build, leaving the project configured for the default (plain ESP32) target, causing a chip mismatch.

**6.** Register a GPIO interrupt (ISR) on the button pin's edge (e.g. falling edge for an active-low button). Inside the ISR, do the minimum possible — call an ISR-safe function like `xTaskNotifyGiveFromISR()` (or `xSemaphoreGiveFromISR()`) to signal a waiting task, then immediately return from the ISR. A separate, normal-priority task blocks on `ulTaskNotifyTake()` and does the actual button-handling logic (including any debounce delay) outside interrupt context — this avoids both busy-polling and unsafe/slow ISR code.

**7.** voltage = 3000 × 3.3 / 4095 ≈ 2.42V

**8.** `printf()` is a relatively slow, potentially blocking operation (it may involve buffering/locking) that's unsafe and far too slow to run inside an ISR, where code must execute in microseconds and cannot block. The correct pattern is to have the ISR only signal a task (via an ISR-safe semaphore/notification call), and let that separate task do the `printf()` (or any other slower work) safely in normal task context.

**9.** **BLE** — the device is battery-powered (BLE's very low power draw between transmissions is ideal for infrequent 6-hour reporting), it only needs to communicate with a nearby phone (BLE's short range is sufficient and appropriate), and there's no internet/cloud requirement (ruling out needing full Wi-Fi). Wi-Fi would be needlessly power-hungry for this use case, and ESP-NOW is designed for device-to-device (not device-to-phone) communication.

**10.** Because Wi-Fi connection in ESP-IDF is asynchronous and event-driven — `esp_wifi_connect()` only *initiates* the connection attempt; the actual outcome (success, IP address assignment, or failure/retry) happens later and is delivered via the ESP-IDF event loop (`WIFI_EVENT_STA_CONNECTED`, `IP_EVENT_STA_GOT_IP`, disconnect events), so checking synchronously right after the call would almost always incorrectly report "not connected yet."

**11.** I2C: all 8 sensors can share just 2 wires (SCL + SDA) total, regardless of count, since they're distinguished by unique bus addresses — total GPIO cost ≈ 2 pins. SPI: each of the 2 SPI devices needs its own dedicated CS line in addition to the 3 shared lines (SCLK, MOSI, MISO) — total GPIO cost ≈ 3 + 2 = 5 pins. So I2C scales much better in pin count as device count grows, which is exactly why it's preferred for many low-speed sensors sharing a bus.

**12.** At long cable lengths and/or high baud rates, UART's simple two-wire electrical signaling becomes more susceptible to signal degradation, noise pickup, and timing skew relative to the receiver's sampling clock (since there's no shared clock line to keep both ends synchronized against drift) — this is a known limitation of standard UART, which is why long-distance serial links typically use differential signaling standards (like RS-485) instead of straight TTL UART.

**13.** (1) Enable Secure Boot on the device so it only ever executes firmware signed with the developer's private key — this is the last line of defense even if every other step is compromised. (2) The OTA update server delivers the new firmware image over an HTTPS/TLS connection, so the transfer itself can't be eavesdropped on or tampered with in transit (man-in-the-middle protection). (3) Before accepting the newly downloaded image, the bootloader verifies its cryptographic signature (the same mechanism Secure Boot uses at every boot) — so even if an attacker somehow compromised the OTA server or intercepted/replaced the download despite TLS, an unsigned or wrongly-signed image is rejected and the device refuses to boot into it. Each step covers a different attack angle: TLS covers the transit, signing+Secure Boot covers the content's authenticity — you need both, not either alone.
