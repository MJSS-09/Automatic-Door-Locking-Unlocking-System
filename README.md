# 🔐 Automatic Door Locking and Unlocking System

*A Sensor-Based Smart Access Control System using PIR, Ultrasonic, and IR Break-Beam Sensors*

**Author:** Mallampally Jayantha Siva Srinivas
**Field:** B.Tech, Electronics and Communication Engineering
**Date:** July 2026

---

## 📖 Overview

Traditional locking mechanisms rely on manual keys or keypads, which can be lost, duplicated, or forgotten. This project implements an **Automatic Door Locking and Unlocking System** that uses sensor-based detection to identify the presence of a person or object near a door, and automatically actuates a servo motor to lock/unlock it — with real-time status feedback on an OLED display.

Three sensing approaches were designed, simulated, and compared:

| Sensor | Platform |
|---|---|
| PIR (Passive Infrared) | Tinkercad Circuits |
| Ultrasonic (HC-SR04) | Cirkit Designer |
| IR Break-Beam | Cirkit Designer |

---

## 🎯 Objectives

- Design and simulate automatic door control using three different sensor technologies.
- Compare PIR, Ultrasonic, and IR sensors to identify the most appropriate sensor for different regions/entry points of a house.
- Provide real-time visual feedback of door status (Opened/Closed) via OLED.
- Control a servo motor to physically represent the door's lock/unlock mechanism.

---

## 🧭 Choosing the Right Sensor for the Right Region

The key factor in sensor selection isn't just detection range — it's how much uncontrolled foot traffic passes near that door.

| House Region | Recommended Sensor | Reason |
|---|---|---|
| Ground Floor Main Entrance (high foot traffic) | Ultrasonic (HC-SR04) | High passerby volume makes PIR/IR unreliable. Ultrasonic only triggers within a fixed distance threshold (e.g. < 15 cm), preventing false unlocking. |
| Garage Door / Gate | Ultrasonic (HC-SR04) | Detects the exact distance of a vehicle before opening, avoiding accidental triggers. |
| Low-Traffic Interior Room Door | PIR Sensor | Controlled, low, known foot traffic — wide-area convenience matters more than strict precision. |
| Narrow Interior Doorway / Cabinet / Locker | IR Break-Beam | Reliable binary (beam broken/clear) detection for narrow, single-file passages. |
| Upper-Floor Balcony / Low-Footfall Backyard | PIR Sensor | Not exposed to public street traffic, so PIR's wider detection cone is not a security gap. |

### Comparative Summary

| Parameter | PIR Sensor | Ultrasonic Sensor | IR Break-Beam |
|---|---|---|---|
| Detection Principle | Passive infrared / body heat | Sound wave echo (time-of-flight) | IR light beam interruption |
| Range | ~6–7 m, wide cone | 2 cm – 400 cm, narrow beam | Depends on beam length |
| Precision | Low (area-based) | High (exact distance in cm) | High (binary: blocked/clear) |
| False-Trigger Risk | High in busy areas | Low — distance threshold filters passersby | Low, only within beam path |
| Affected By | Heat sources, pets, sunlight | Soft/sound-absorbing surfaces | Ambient IR light, misalignment |
| Ideal Region | Low-traffic interior/upper floors | Ground floor entrance, garages, gates | Narrow, low-traffic doorways/cabinets |
| Cost | Low | Low–Medium | Low |

---

## 🛠️ Components Required

### Hardware

| Component | Qty | Purpose |
|---|---|---|
| Arduino Uno | 1 | Main microcontroller |
| PIR Motion Sensor (HC-SR501) | 1 | Motion detection for entrance config |
| Ultrasonic Sensor (HC-SR04) | 1 | Distance measurement |
| IR Break-Beam Sensor | 1 | Beam-interruption presence detection |
| SG90 Micro Servo Motor | 1 | Door lock/unlock actuator |
| 0.96" OLED Display (SSD1306, I2C, 128x64) | 1 | Real-time status display |
| Red & Green LEDs | 2 | Lock/unlock indicators (PIR config) |
| 220Ω Resistors | 2 | Current limiting for LEDs |
| Breadboard | 1 | Prototyping |
| Jumper Wires (M-M) | As required | Circuit connections |
| USB Cable / 5V Power Supply | 1 | Power source |

### Software

- **Tinkercad Circuits** — PIR-based circuit design & simulation
- **Cirkit Designer** — Ultrasonic and IR-based circuit design & simulation
- **Adafruit SSD1306** & **Adafruit GFX** libraries — OLED driving
- **Servo.h** — Servo motor control
- **Wire.h** — I2C communication

---

## ⚙️ System Design — Sense → Decide → Act → Display

- **Sense:** Active sensor (PIR/Ultrasonic/IR) continuously monitors surroundings.
- **Decide:** Arduino evaluates the sensor reading against a threshold (motion HIGH for PIR, distance < 10–15 cm for Ultrasonic, beam-broken LOW for IR).
- **Act:** Servo rotates to 90° (open/unlocked) or 0° (closed/locked).
- **Display:** OLED/Serial Monitor shows "Door is Opened" / "Door is Closed", plus live distance for the ultrasonic variant.

---

## 1️⃣ PIR Sensor Based Design (Tinkercad)

Wide-area motion detection, ideal for a home's main entrance. Uses red/green LEDs to indicate locked/unlocked states.

### Circuit Connections

| Component | Pin | Arduino Pin |
|---|---|---|
| PIR Sensor | OUT | Digital Pin 9 |
| PIR Sensor | VCC/GND | 5V/GND |
| Servo Motor | Signal | Digital Pin 6 |
| Servo Motor | VCC/GND | 5V/GND |
| Red LED | Anode (via 220Ω) | Digital Pin 2 |
| Green LED | Anode (via 220Ω) | Digital Pin 4 |

---

## 2️⃣ Ultrasonic Sensor Based Design (Cirkit Designer)

Suited for garages/gates where the exact distance of an approaching object/vehicle matters. Displays live distance on OLED.

### Circuit Connections

| Component | Pin | Arduino Pin |
|---|---|---|
| HC-SR04 | Trig | Digital Pin 8 |
| HC-SR04 | Echo | Digital Pin 7 |
| HC-SR04 | VCC/GND | 5V/GND |
| Servo Motor | Signal | Digital Pin 3 |
| Servo Motor | VCC/GND | 5V/GND |
| OLED Display | SDA/SCL | A4/A5 |
| OLED Display | VCC/GND | 5V/GND |

---

## 3️⃣ IR Break-Beam Sensor Based Design (Cirkit Designer)

Best suited for narrow doorways, cabinets, or lockers requiring simple, reliable line-of-sight detection. Two versions tested: Serial Monitor (initial) and OLED (final).

### Circuit Connections

| Component | Pin | Arduino Pin |
|---|---|---|
| IR Break-Beam Sensor | OUT | Digital Pin 7 |
| IR Break-Beam Sensor | VCC/GND | 5V/GND |
| Servo Motor | Signal | Digital Pin 3 |
| Servo Motor | VCC/GND | 5V/GND |
| OLED Display | SDA/SCL | A4/A5 |
| OLED Display | VCC/GND | 5V/GND |

---

## 📊 Results and Discussion

All three sensor-based configurations were successfully simulated and verified.

| Sensor Variant | Trigger Condition | Door State Observed | Feedback Method |
|---|---|---|---|
| PIR | Motion detected (HIGH) | Servo → 90°, green LED ON | LED indicators |
| PIR | No motion (LOW) | Servo → 0°, red LED ON | LED indicators |
| Ultrasonic | Distance < 15 cm | Servo → 90°, "Opened" shown | OLED (with live distance) |
| Ultrasonic | Distance ≥ 15 cm | Servo → 0°, "Closed" shown | OLED (with live distance) |
| IR Break-Beam | Beam interrupted (LOW) | Servo → 90°, "Opened" printed | OLED / Serial Monitor |
| IR Break-Beam | Beam clear (HIGH) | Servo → 0°, "Closed" printed | OLED / Serial Monitor |

### ✅ Advantages

- Low-cost, easily reproducible design using widely available components.
- Modular sensor approach — the same servo/OLED logic is reused across all three sensor types.
- Real-time visual feedback improves usability over a purely mechanical lock.
- Flexibility to select the most appropriate sensor per house region.

### ⚠️ Limitations

- PIR sensors can produce false triggers from pets or nearby heat sources.
- Ultrasonic sensors may give inconsistent readings on soft/sound-absorbing surfaces.
- IR break-beam sensors require precise physical alignment between transmitter and receiver.
- Current design is simulation-based; real-world deployment requires weatherproofing and a manual override for safety.

---

## 🔮 Future Scope

- Keypad or RFID-based override for manual access.
- Integration with a mobile app via Wi-Fi/Bluetooth (e.g., ESP32) for remote monitoring.
- Battery backup with solar charging for outdoor deployments.
- Data-logging feature to record entry/exit events for security auditing.

---

## 📚 References

- [Arduino Official Documentation](https://docs.arduino.cc)
- [Adafruit SSD1306 & GFX Library Documentation](https://learn.adafruit.com/monochrome-oled-breakouts)
- [Tinkercad Circuits](https://www.tinkercad.com/circuits)
- [Cirkit Designer](https://app.cirkitdesigner.com)
- HC-SR04 Ultrasonic Sensor Datasheet
- Parallax PIR Sensor (555-28027) Datasheet

---

## 👤 Author

**Mallampally Jayantha Siva Srinivas**
B.Tech, Electronics and Communication Engineering
