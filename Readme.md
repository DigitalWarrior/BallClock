# ESP32-C3 Stepper Motor Controller

Created by: Claude (Anthropic)
Version: 1.0.0
Date: 2024-12-30

ESP32-C3 based controller for 28BYJ-48 stepper motor with NTP time synchronization and automatic DST handling.

## Features
- Stepper motor control (2 RPM)
- NTP time sync with DST adjustment
- Offline time keeping
- Dual LED status indicators
- Auto WiFi reconnection
- Serial monitoring (115200 baud)

## Hardware Requirements
- ESP32-C3 microcontroller
- 28BYJ-48 5V stepper motor
- ULN2003 driver board
- 5V power supply

### Pinout
```
Motor Driver (ULN2003):
┌────────────┬───────────┐
│ ULN2003    │ ESP32-C3  │
├────────────┼───────────┤
│ IN1        │ GPIO 2    │
│ IN2        │ GPIO 3    │
│ IN3        │ GPIO 4    │
│ IN4        │ GPIO 5    │
│ VCC        │ 5V        │
│ GND        │ GND       │
└────────────┴───────────┘

Status LEDs:
- System Status: GPIO 13 (onboard LED)
- NTP Status: GPIO 12
```

### Status Indicators
```
System LED (GPIO 13):
┌──────────────────┬────────────────────────┐
│ Pattern          │ Status                 │
├──────────────────┼────────────────────────┤
│ Solid ON         │ Standard Time          │
│ Blink 2x/sec     │ DST Active            │
│ Blink 1x/2sec    │ No WiFi Connection    │
└──────────────────┴────────────────────────┘

NTP LED (GPIO 12):
┌──────────────────┬────────────────────────┐
│ Pattern          │ Status                 │
├──────────────────┼────────────────────────┤
│ Blink 1x/sec     │ NTP Sync Success      │
│ Fast Blink       │ NTP Sync Failed       │
└──────────────────┴────────────────────────┘
```

### Serial Output
```
115200 baud rate
- Boot confirmation
- WiFi connection status
- NTP sync attempts and results
- Current time (HH:MM:SS)
- Time source (NTP/Internal)
```

## Software Dependencies
- Arduino IDE
- ESP32 Board Support
  ```
  https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
  ```
- Libraries:
  - WiFi.h (built-in)
  - NTPClient
  - Timezone (by Jack Christensen)

## Setup
1. Install Arduino IDE
2. Add ESP32 board support URL in preferences
3. Install required libraries
4. Board settings:
   - Board: "ESP32C3 Dev Module"
   - Flash Mode: "DIO"
   - Upload Speed: 460800
   - Flash Size: 4MB
   - Partition: Default

## Configuration
```cpp
// WiFi Settings
const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";
const char* ntpServer = "132.163.96.1";  // time.nist.gov

// Timezone (Example: US Eastern)
TimeChangeRule EDT = {"EDT", Second, Sun, Mar, 2, -240};  // UTC-4h
TimeChangeRule EST = {"EST", First, Sun, Nov, 2, -300};   // UTC-5h
```

## Operation
- Auto-connects to WiFi on boot
- Syncs with NTP server hourly when online
- Maintains time internally when offline
- Automatically adjusts for DST
- Updates motor position every 30ms for 2 RPM

## License
MIT License

## Acknowledgments
- ESP32 Arduino Core
- Jack Christensen (Timezone library)