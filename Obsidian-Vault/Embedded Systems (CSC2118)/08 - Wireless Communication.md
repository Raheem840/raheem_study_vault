---
topic: "Outline Topic 6 — Wireless Communication"
source: "ESP-IDF Wi-Fi/BLE/ESP-NOW docs (free, docs.espressif.com)"
course: "[[00 - Course Map|CSC 2118 - Embedded Systems]]"
tags: [embedded, csc2118, exam-material, wireless]
---

# Wireless Communication (ESP32-S3)

## 1. The three wireless options on ESP32-S3
| Technology | Range | Data rate | Power use | Typical use |
|---|---|---|---|---|
| **Wi-Fi** (802.11 b/g/n, 2.4GHz) | ~50-100m indoors | High (Mbps) | High | Internet connectivity, cloud data upload, web server on the device |
| **Bluetooth Low Energy (BLE)** | ~10-30m | Low (Kbps) | Very low | Phone-to-device pairing, wearables, low-power sensors |
| **ESP-NOW** | ~100-200m (line of sight) | Moderate | Low | Direct device-to-device messaging, no router/internet needed, very low latency |

## 2. Wi-Fi modes
- **Station (STA) mode**: the ESP32 connects to an existing Wi-Fi router/access point as a client — most common for IoT devices needing internet access.
- **Access Point (AP) mode**: the ESP32 itself becomes a Wi-Fi hotspot other devices connect to — used for initial device setup ("captive portal" configuration) when there's no known network yet.
- **AP+STA mode**: both simultaneously (e.g. device is connected to the internet AND still broadcasting its own config network).

### Basic Wi-Fi station connection flow (conceptual, exam-relevant sequence)
1. Initialize the underlying network stack (`esp_netif_init()`) and the default event loop (`esp_event_loop_create_default()`).
2. Configure Wi-Fi in station mode with SSID/password (`esp_wifi_set_config()`).
3. Start Wi-Fi (`esp_wifi_start()`), then connect (`esp_wifi_connect()`).
4. Register event handlers for `WIFI_EVENT_STA_CONNECTED`, `IP_EVENT_STA_GOT_IP`, and disconnect events — **Wi-Fi is asynchronous/event-driven**, not a blocking call-and-wait.
5. Once an IP address is obtained, your application tasks can use HTTP/MQTT/etc.

> **Exam trap**: You cannot safely assume Wi-Fi is connected right after calling `esp_wifi_connect()` — you must wait for the actual `IP_EVENT_STA_GOT_IP` event, since the connection is asynchronous and can also fail/retry.

## 3. Why Wi-Fi connectivity is handled as FreeRTOS tasks + events
The Wi-Fi/BLE protocol stacks themselves run as their own FreeRTOS tasks in the background (often pinned to Core 0), communicating with your application code via the **ESP-IDF event loop** (a publish/subscribe system) rather than by blocking function calls — this ties directly back to the task/queue/notification concepts from Parts A–C, just applied by Espressif's own libraries instead of your own code.

## 4. Bluetooth Low Energy (BLE) — the basics
BLE communication centers on **GATT** (Generic Attribute Profile):
- A **Server** (often the ESP32) exposes **Services**, each containing one or more **Characteristics** (a piece of data, e.g. "battery level" or "temperature reading") that a **Client** (e.g. a phone app) can read, write, or subscribe to notifications from.
- Much lower power than Wi-Fi because BLE devices spend most of their time in a low-power "advertising" or sleep state, only briefly waking to transmit.

## 5. ESP-NOW — direct peer-to-peer messaging
A lightweight, connectionless protocol built by Espressif on top of Wi-Fi's physical layer, but **without needing a router or full Wi-Fi connection/handshake** — devices exchange short messages directly using just each other's MAC address. Very low latency and low power compared to full Wi-Fi, popular for sensor networks and simple device-to-device control (e.g. a remote controlling several ESP32 nodes directly).

## 6. Choosing the right wireless tech (a classic design-question pattern)
- Need internet/cloud access → **Wi-Fi**
- Need lowest power, short-range, phone pairing → **BLE**
- Need many nodes talking directly to each other fast, no internet needed → **ESP-NOW**

## 7. Quick self-test
1. Name the three wireless technologies on ESP32-S3 and one defining use case for each.
2. Distinguish Wi-Fi Station mode from Access Point mode.
3. Why is checking for `IP_EVENT_STA_GOT_IP` important instead of assuming connection right after `esp_wifi_connect()`?
4. Define GATT's Service/Characteristic model in BLE, and name the two roles (Server/Client).
5. What makes ESP-NOW lower-latency and lower-power than a full Wi-Fi connection for device-to-device messaging?
6. If you needed 50 sensor nodes to relay readings to each other directly without any router present, which wireless technology fits best, and why?
