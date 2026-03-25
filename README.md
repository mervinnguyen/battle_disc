# Battle Disc

> Embedded Ping Pong Game on an AtMega328P — I2C OLED display, motion controls, designed for scale.

---

## Overview

Battle Disc is a two-player Pong implementation built for a custom hardware platform. Physical motion controls map to in-game paddle movement, rendered in real time on an I2C OLED display. The project was developed with a production target of 500+ units for deployment in after-school STEM programs.

---

## Key Achievements

- **Led a 2-person engineering team** through full-cycle development — from schematic design to firmware deployment via PlatformIO.
- **Designed custom PCB** with I2C communication bus driving a 128×64 OLED display for real-time game rendering.
- **Implemented motion control input** in C++ using Adafruit libraries, translating physical gestures into paddle actuation.
- **Scoped for production deployment** across 500+ embedded systems targeting K–12 after-school programs.

---

## Hardware

| Component | Role |
|---|---|
| Arduino Uno | Main MCU |
| 128×64 OLED Display | I2C — game rendering |
| Motion/Input Controllers | Player paddle input |
| Breadboard Design | Integrated schematic, routing, and component layout |

---

## Software

| Layer | Detail |
|---|---|
| Language | C++ |
| Framework | Arduino + PlatformIO |
| Libraries | Adafruit GFX, Adafruit SSD1306 |
| IDE | Visual Studio Code |
| PCB Design | Altium Designer |

---

## Architecture
