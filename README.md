# **ESP32 Zigbee Network Scanner**

A simple and powerful example demonstrating how to scan Zigbee networks using the **ESP-IDF Zigbee stack** (Arduino Core for ESP32).
This example shows how to configure the ESP32 as a Zigbee device (Coordinator, Router, or End Device) and perform an active network scan across available Zigbee channels.

Created by **Jan Procházka (P-R-O-C-H-Y)**.
Updated and documented for GitHub.

---

## ⭐ **Features**

* Scan for all nearby Zigbee networks
* Lists:

  * PAN ID
  * Logical channel
  * Permit-joining status
  * Router & end-device capacity
  * Extended PAN ID (64-bit)
* Continuous scanning loop
* Automatically handles scan completion / failure
* Works in **Zigbee Router** or **Zigbee End Device** mode
* Minimal example to understand Zigbee scanning with ESP32

---

## 📦 **Requirements**

### **Hardware**

* ESP32 (with Zigbee-enabled SoC such as ESP32-H2, ESP32-C6, ESP32-H4)
* USB cable
* Zigbee networks around (router, coordinator, smart bulbs, etc.)

### **Software**

* Arduino IDE 2.x
* ESP32 Arduino Core **v3.0 or newer**
* Zigbee-enabled board with:

  * `Tools → Zigbee Mode`
  * `Tools → Partition Scheme → Zigbee`

---

## ⚙️ **Zigbee Mode Configuration**

Before uploading, configure:

**Arduino IDE → Tools:**

| Setting          | Value                                               |
| ---------------- | --------------------------------------------------- |
| Zigbee Mode      | *Zigbee Router (ZC/ZR)* or *Zigbee End Device (ED)* |
| Partition Scheme | *Zigbee*                                            |
| Board            | ESP32-H2 DevKit / ESP32-C6 DevKit or similar        |

> ⚠️ If you do not select a Zigbee mode, the sketch will show this error:

```
#error "Zigbee device mode is not selected in Tools->Zigbee mode"
```

---

## 🚀 **How the Code Works**

### **1. Initialize Zigbee Stack**

```cpp
Zigbee.begin(role);
```

No endpoints are created—this example is scanning only.

### **2. Start Scanning**

```cpp
Zigbee.scanNetworks();
```

### **3. Check Scan Status**

```cpp
int16_t status = Zigbee.scanComplete();
```

Possible values:

| Status            | Meaning                        |
| ----------------- | ------------------------------ |
| `ZB_SCAN_RUNNING` | Still scanning                 |
| `ZB_SCAN_FAILED`  | Failed, restarts automatically |
| `>= 0`            | Number of networks found       |

### **4. Print Networks**

Example output formats:

```
Nr | PAN ID | CH | Permit Joining | Router Capacity | End Device Capacity | Extended PAN ID
 1 | 0x1234 | 11 | Yes            | Yes             | No                  | 11:22:33:44:55:66:77:88
```

---

## 📋 **Sample Serial Output**

```
Setup done
Scan done
3 networks found:
Nr | PAN ID | CH | Permit Joining | Router Capacity | End Device Capacity | Extended PAN ID
 1 | 0x1A2B | 12 | Yes            | Yes             | Yes                 | 2A:FF:3D:18:A1:00:4C:9D
 2 | 0x55AA | 20 | No             | Yes             | No                  | A8:41:11:DD:23:89:7F:20
 3 | 0x99CC | 25 | Yes            | No              | No                  | 10:4F:BB:01:AA:00:FF:EE
```

The scanner then automatically restarts and scans again.

---

## 📁 **Code Structure**

```
zigbee_scan/
├── zigbee_scan.ino        # Main example
└── README.md              # This documentation
```

---

## 🧠 **How It Works Internally**

* The ESP32 sends active Zigbee scan requests on all default channels.
* Each nearby Zigbee Coordinator or Router responds with beacon details.
* Results are stored in `zigbee_scan_result_t` objects.
* After printing results, the memory is freed using:

```cpp
Zigbee.scanDelete();
```

---

## 🔄 **Continuous Scanning**

The example is designed to scan forever:

```cpp
printScannedNetworks(ZigbeeScanStatus);
Zigbee.scanNetworks();  // start over
```

You can modify this to:

* Scan once
* Scan on button press
* Scan on command
* Forward results to serial, MQTT, or WiFi

---

## 🪪 **License**

Licensed under the **Apache License 2.0**
Copyright © 2024 Espressif Systems
