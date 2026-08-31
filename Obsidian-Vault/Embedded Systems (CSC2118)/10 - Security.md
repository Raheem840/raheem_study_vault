---
topic: "Outline Topic 8 — Security"
source: "ESP-IDF Security docs (free, docs.espressif.com) — Secure Boot, Flash Encryption, TLS"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material, security]
---

# Security in Embedded Systems (ESP32-S3)

## 1. Why embedded security is a distinct (harder) problem
Unlike servers, embedded devices are often **physically accessible to attackers** (someone can literally hold the chip, probe its pins, or desolder the flash chip), have **limited compute** for heavy cryptography, and are frequently **deployed in the field for years without easy updates** — so security has to be designed in from the start, not patched on later.

## 2. Secure Boot — trusting your own firmware
**Secure Boot** ensures the chip only runs firmware that's been **cryptographically signed** by the developer, preventing an attacker from flashing malicious firmware onto a stolen or physically-accessed device.
- Works via a **chain of trust**: the chip's immutable (burned into silicon/eFuses) first-stage bootloader verifies the signature of the next stage, which verifies the signature of the application firmware, and so on.
- On ESP32-S3, the signing key is generated once and its digest burned into one-time-programmable **eFuses** — after enabling Secure Boot, this cannot be undone (a deliberate, permanent hardware lock).

## 3. Flash Encryption — protecting firmware/data at rest
Even if Secure Boot stops *unauthorized* firmware from running, an attacker with physical access could still **read** the flash chip directly and steal your source code, Wi-Fi credentials, or proprietary algorithms. **Flash Encryption** encrypts the contents of external flash so it's unreadable without the chip's own (hardware-generated, never exposed) encryption key.
- Best practice: **enable both Secure Boot AND Flash Encryption together** — Secure Boot alone doesn't stop someone reading your flash contents; Flash Encryption alone doesn't stop someone flashing different firmware.

## 4. NVS Encryption (Non-Volatile Storage)
ESP-IDF's **NVS** partition is where you typically store settings like Wi-Fi credentials and calibration data. NVS encryption (built on top of Flash Encryption) specifically protects this key-value data at rest.

## 5. Secure network communication — TLS/SSL
When an embedded device talks to a cloud server (over Wi-Fi, per [[08 - Wireless Communication]]), the connection itself needs protecting:
- **TLS (Transport Layer Security)** encrypts data in transit and verifies the server's identity via **certificates**, preventing eavesdropping and man-in-the-middle attacks.
- ESP-IDF includes **mbedTLS** for this — used automatically when you connect via `https://` or `mqtts://` instead of the unencrypted `http://`/`mqtt://`.
- Embedded devices, being resource-constrained, often use **lighter cipher suites** than a full server, but the core guarantee (confidentiality + authentication) is the same idea.

## 6. OTA (Over-The-Air) update security
Since embedded devices are often deployed for years, remote firmware updates (OTA) are common — but an insecure OTA mechanism is a huge attack surface (an attacker could push malicious "updates"). Secure OTA requires: the update is fetched over TLS (confidentiality/integrity in transit) AND the new firmware image is itself signed and verified by Secure Boot before being accepted (so a compromised OTA server still can't push unsigned firmware).

## 7. Common attack surfaces to know (typical exam list)
| Attack surface | Risk | Mitigation |
|---|---|---|
| Physical access to flash chip | Firmware/secrets extraction | Flash Encryption |
| Malicious firmware flashing | Full device compromise | Secure Boot |
| Network eavesdropping | Credential/data theft | TLS |
| Insecure OTA updates | Remote malicious firmware push | Signed firmware + TLS-delivered updates |
| Hardcoded Wi-Fi/API credentials in source | Credential leakage if code/binary is examined | NVS + Flash Encryption, avoid hardcoding secrets |
| Debug/JTAG interfaces left enabled in production | Full memory/register access for an attacker | Disable JTAG / UART download mode in production builds |

## 8. Quick self-test
1. Why is embedded security harder in some ways than server security — name two reasons.
2. What does Secure Boot actually verify, and what stops an attacker from just disabling it?
3. Why do you need Flash Encryption in addition to Secure Boot, not instead of it?
4. What does TLS protect against, and what ESP-IDF library implements it?
5. What two conditions make an OTA update mechanism secure?
6. Name three concrete attack surfaces on a deployed IoT device and their mitigations.
