---
course: CSC 2118 Embedded and Real-time Systems
lecturer: Mary Nsabagwa
room: Hi-Train-Lab
day: Thursday & Friday, 14:00–16:00
textbook: See 'References' below — no single set textbook; using official free documentation + your lecture slides
semester: 2026/2027 Semester I
---

# CSC 2118 — Embedded and Real-Time Systems

> You said the lecturer isn't really teaching this well, so treat this vault as the actual course — read the notes as if I'm your lecturer, do the Anki daily, and hit the practice questions before any test.

## Official course outline → what we're building
| # | Outline topic | Status | Note |
|---|---|---|---|
| 1 | Introduction to embedded systems | Done | [[02 - Introduction to Embedded Systems]] |
| 2 | ESP32-S3 microcontroller | Done | [[02b - ESP32-S3 Microcontroller]] |
| 3 | Introduction to FreeRTOS | Done | [[03 - FreeRTOS Fundamentals (Tasks and Scheduling)]], [[04 - FreeRTOS Inter-Task Communication]], [[05 - FreeRTOS Timers and Task Notifications]] |
| 4 | Setting up ESP-IDF | Done | [[06 - Setting up the ESP-IDF Environment]] |
| 5 | I/O and sensor interfacing | Done | [[07 - IO and Sensor Interfacing]] |
| 6 | Wireless communication | Done | [[08 - Wireless Communication]] |
| 7 | Serial communication | Done | [[09 - Serial Communication]] |
| 8 | Security | Done | [[10 - Security]] |
| — | **Prerequisite: C for embedded systems** (not in outline, but you can't do any of the above without it) | Done | [[01 - C Programming Foundations for Embedded Systems]] |

**Full syllabus is now covered.** Everything below has a matching Anki deck and set of practice questions in the repo.

## References (all free — no paid books)
1. **FreeRTOS official docs & free reference manual** — freertos.org/Documentation/RTOS_book.html — "Mastering the FreeRTOS Real Time Kernel" is a free official PDF. This is the authoritative source, written by the people who built FreeRTOS — better than any third-party Amazon book.
2. **Espressif ESP-IDF Programming Guide** — docs.espressif.com/projects/esp-idf — official, free, and specific to your exact chip (ESP32-S3) and the API calls your lecturer's slides use (`gpio_set_level`, `xTaskCreate`, etc.).
3. **Your own lecture slides** (`03_-_FreeRTOS.pdf`) — genuinely good content, just under-explained. I've expanded on them below rather than replaced them.
4. For plain C fundamentals: **"Modern C" by Jens Gustedt** — free PDF (author-hosted), solid on pointers/structs/memory which is 90% of what trips people up in embedded C.

I did **not** point you to any of the Amazon "ESP32 FreeRTOS" books that turned up in search — most of the current crop are AI-generated print-on-demand filler with no real technical vetting. The official docs above are better, free, and more accurate.

## Links
- [[01 - C Programming Foundations for Embedded Systems]]
- [[02 - Introduction to Embedded Systems]]
- [[02b - ESP32-S3 Microcontroller]]
- [[03 - FreeRTOS Fundamentals (Tasks and Scheduling)]]
- [[04 - FreeRTOS Inter-Task Communication]]
- [[05 - FreeRTOS Timers and Task Notifications]]
- [[06 - Setting up the ESP-IDF Environment]]
- [[07 - IO and Sensor Interfacing]]
- [[08 - Wireless Communication]]
- [[09 - Serial Communication]]
- [[10 - Security]]
