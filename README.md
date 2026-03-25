# Battle Disc 2.0 — Miniaturized Pong

> Two-player embedded Ping Pong on a 128×64 OLED — real-time game loop, CPU AI, and push-button controls on Arduino Uno.

---

## Overview

Battle Disc 2.0 is a self-contained arcade game running on an Arduino Uno. Two push buttons control the player paddle; a deterministic CPU opponent tracks the ball via midpoint logic. All game state — ball physics, collision detection, score, and paddle positions is updated in a single non-blocking loop and rendered over I2C to a 128×64 SSD1306 OLED at a fixed frame cadence.

**Deployment target:** 50+ units for K–12 after-school STEM programs to educate next generation of engineers in Embedded Systems.

---

## Hardware

| Component | Spec / Role |
|---|---|
| Arduino Uno | ATmega328P — game loop, I2C master |
| SSD1306 OLED | 128×64 px, I2C (0x3C) — display output |
| Push Buttons (×2) | Digital pins 2 & 3 — UP / DOWN player input |
| 10 kΩ Resistors | Pull-down — debounce stabilization |
| Breadboard + Wire | Prototyping (Fritzing schematic included) |

---

## Software

| Layer | Detail |
|---|---|
| Language | C++ (Arduino framework) |
| Build System | PlatformIO |
| Libraries | Adafruit GFX, Adafruit SSD1306 |
| Schematic | Fritzing |
| IDE | Visual Studio Code |

---

## Architecture

```
Push Buttons (GPIO 2, 3)
        │
        ▼
  Arduino Uno — Main Game Loop (loop())
  ├── Ball update (every ballRate ms)
  │   ├── Position increment
  │   ├── Wall bounce (y-axis)
  │   ├── Paddle collision (x-axis)
  │   └── Score check → restartGame() on win/loss
  ├── Paddle update (every paddleRate ms)
  │   ├── CPU AI — midpoint tracking (cpu_y vs ball_y)
  │   └── Player — UP/DOWN button state
  └── Dirty flag → display.display() only when changed
        │
        ▼
  I2C Bus (Wire, 0x3C) → SSD1306 OLED
  ├── Court boundary (drawRect)
  ├── Ball (drawPixel — erase old, draw new)
  ├── Paddles (drawFastVLine — erase old, draw new)
  └── Score HUD (fillRect clear → print)
```

---

## Game Logic

**Ball physics** — The ball moves one pixel per `ballRate` ms in both axes. On wall contact the relevant direction component is negated; on paddle contact the x-component flips and the ball steps forward two pixels to avoid tunneling.

**CPU AI** — Each `paddleRate` tick, the CPU paddle midpoint is compared to `ball_y`. The paddle steps ±1 px toward the ball — simple but effective at the chosen update rate.

**Scoring** — First to 2 points wins. On score, the ball resets to center (x=64) and direction is reversed. On game end, a result screen displays for 1 s before `restartGame()` re-initializes all state.

**Render strategy** — A `bool update` dirty flag ensures `display.display()` is only called when ball or paddle state changed, reducing unnecessary I2C transfers.

---

## Getting Started

### Requirements
- PlatformIO (VS Code extension)
- Adafruit GFX Library
- Adafruit SSD1306 Library

### Wiring

```
Arduino      SSD1306
A4    ──────  SDA
A5    ──────  SCL
Pin 4 ──────  RESET
3.3V  ──────  VCC
GND   ──────  GND

Arduino      Buttons
Pin 2 ──────  UP   (pull-down to GND)
Pin 3 ──────  DOWN (pull-down to GND)
```

## Screenshots

**Loading Screen**

<img src="https://i.imgur.com/I8a9Z8k.jpg" width="45%" />

**In Game**

<img src="https://i.imgur.com/4Y7TW9o.jpg" width="45%" />

---

## Limitations

- Ball travels exactly 1 px/tick — no angle variation on paddle hit
- CPU AI is deterministic; difficulty is purely a function of `ballRate` / `paddleRate`
- `display.begin()` is called inside `restartGame()` — should be moved to `setup()` only

---

## Roadmap

- [ ] Wireless multiplayer via HC-05 Bluetooth
- [ ] Difficulty scaling (ball speed ramp)
- [ ] EEPROM high score persistence
- [ ] Production enclosure design

---

## Authors

Ben Soo · Mervin Nguyen — September 2023
