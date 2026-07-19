# Smart Dustbin 🗑️
### Automatic Wet/Dry Waste Segregation with OLED-Based Real-Time Monitoring

An Arduino Uno–based embedded system that automates touchless lid access, wet/dry waste sorting, and bin fill-level monitoring — with live status shown on an OLED display. Designed and validated entirely through circuit simulation.

---

## Overview

Conventional dustbins require physical contact, offer no waste separation, and give no warning before overflowing. This project solves all three problems with a single low-cost microcontroller build:

- **Touchless lid** — opens automatically when a hand/object is detected
- **Automatic wet/dry sorting** — a soil moisture sensor classifies waste and a servo-driven flap routes it to the correct compartment
- **Fill-level monitoring** — a third ultrasonic sensor tracks how full the bin is and triggers a buzzer alert
- **Real-time OLED display** — shows lid state, waste type, fill %, and alerts at a glance

## Hardware Used

| Component | Qty |
|---|---|
| Arduino Uno | 1 |
| HC-SR04 Ultrasonic Sensor | 3 |
| Servo Motor (SG90 or similar) | 2 |
| Soil Moisture Sensor | 1 |
| Piezo Buzzer | 1 |
| SSD1306 OLED Display (128x64, I2C) | 1 |
| Breadboard + jumper wires | — |

## Pin Mapping

| Module | Function | Arduino Pin |
|---|---|---|
| US1 – Lid | TRIG / ECHO | D6 / D7 |
| US2 – Waste Detect | TRIG / ECHO | D8 / D9 |
| US3 – Fill Level | TRIG / ECHO | D10 / D12 |
| Servo 1 (Lid) | Signal | D3 |
| Servo 2 (Sorting Flap) | Signal | D5 |
| Soil Moisture Sensor | Analog Out | A0 |
| Piezo Buzzer | Signal | D11 |
| OLED (SSD1306, I2C) | SDA / SCL | A4 / A5 |

## How It Works

1. **Lid control** — the first ultrasonic sensor watches for an approaching hand/object; within threshold range, it opens the lid servo and closes it again after a short delay once the object moves away.
2. **Wet/dry sorting** — the second ultrasonic sensor detects waste falling into the chute. When triggered, the soil moisture sensor is read; below the wet threshold, the flap servo swings to the wet-waste angle, otherwise to the dry-waste angle, then resets.
3. **Fill monitoring** — the third ultrasonic sensor measures distance to the waste surface, converts it to a fill percentage, and switches the buzzer on once the configured threshold (default 90%) is reached.
4. **Status display** — every loop, the OLED refreshes with the current lid state, last waste type, fill %, and a bin-full warning if applicable.

## Circuit Diagram

See `docs/schematic.png` (or the full project report) for the complete wiring diagram covering all sensors, servos, buzzer, and OLED connections on the shared 5V/GND rails.

## Arduino Sketch

The full sketch (`SmartDustbin.ino`) uses:
- `Servo.h` for the two servo motors
- `Wire.h` + `Adafruit_GFX.h` + `Adafruit_SSD1306.h` for the OLED display

Key tunable constants at the top of the sketch:
```cpp
const int LID_OPEN_DIST_CM   = 15;   // hand distance to trigger lid opening
const int WASTE_DROP_DIST_CM = 10;   // distance to detect waste has been dropped
const int BIN_HEIGHT_CM      = 30;   // total height of bin from sensor to bottom
const int SOIL_WET_THRESHOLD = 500;  // below this analog value = wet
const int FULL_PERCENT_LIMIT = 90;   // buzzer triggers at this fill %
```
Adjust these to match your physical bin dimensions and sensor calibration.

## Simulation Results

Validated in circuit simulation across all operating states:
- ✅ Idle (lid closed, bin empty, status normal)
- ✅ Lid open, awaiting waste
- ✅ Wet waste detected and sorted
- ✅ Dry waste detected and sorted
- ✅ Bin-full alert (lid open and lid closed)

## Advantages

- Touchless, hygienic lid operation
- Reduces contamination between recyclable and biodegradable waste
- Prevents overflow via real-time fill alerts
- Built entirely from low-cost, widely available components
- Modular design — easy to extend with wireless reporting or more waste categories

## Limitations

- Soil moisture sensor is a proxy for wet/dry classification and may misclassify borderline items (e.g., damp paper)
- Ultrasonic readings can be inconsistent with soft or irregularly shaped waste
- Validated only in simulation — a physical build needs an external servo power supply and enclosure design
- No wireless connectivity in the current version
- Bin-height and thresholds are hard-coded and need recalibration for different bin sizes

## Future Scope

- Add ESP8266/ESP32 for Wi-Fi/cloud dashboard reporting
- Add a third sorting category (e.g., e-waste)
- Replace soil moisture sensing with capacitive/IR-based material classification
- Solar charging + battery for outdoor deployment
- GPS + route optimization across a network of bins
- Weatherproof enclosure for public/outdoor deployment

## 🙋 Author

**Galla Indranag**
B.Tech ECE, NRI Institute of Technology, Guntur
Embedded Systems & IoT Engineering (ESSCI-certified), SRM- AP
- GitHub: [@indra675](https://github.com/indra675)
- LinkedIn: [linkedin.com/in/gallaindranag](https://linkedin.com/in/gallaindranag)
