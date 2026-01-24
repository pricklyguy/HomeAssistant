# 🎄 Christmas Kiosk System (2025)

A complete Home Assistant-integrated Christmas display kiosk with timer, rain detection, audio playback, and LED effects.

## Overview

This system uses **two ESP32 devices** working together:
- **Main Controller** (`new-kiosk`) - ESP32-DevKit managing sensors, relays, audio, and communication
- **Timer Display** (`timerkiosk`) - CYD (Cheap Yellow Display) showing countdown timer with touchscreen control

## Hardware

### Main Controller (ESP32-DevKit)
- **Sensors:**
  - DHT22 (Temperature/Humidity)
  - PIR Motion Sensor
  - Rain Detection (Digital + 2x Analog)
  - Voltage Monitoring (VIN)
  - Lid Position Sensors (Open/Closed)

- **Outputs:**
  - WS2812 LED Strip (158 LEDs)
  - 6x Relay Control (Amp, Fan, Tablet Charger, etc.)
  - Linear Actuator (Lid Control)
  - YX5300 Audio Module

### Timer Display (CYD - ESP32-DevKit)
- ILI9341 320x240 TFT Display
- XPT2046 Touchscreen
- UART Communication to Main Controller
- PWM Backlight Control

## Features

### ⏱️ Timer System
- Configurable duration (60-900 seconds)
- Physical button start
- Touchscreen start on CYD display
- Live countdown display with color-coded warnings
- Auto-shutoff when timer expires

### 🌧️ Rain Protection
- Automatic shutdown on rain detection
- Lid closes automatically
- Audio warning announcement
- Flashing blue LED effect
- Auto-recovery when rain clears (during show hours)

### 🎵 Audio System
- YX5300 MP3 player with SD card
- Pre-recorded voice messages:
  - Welcome message
  - Thank you message
  - Rain warning
  - Rain clear
  - Sequential motion-triggered greetings (tracks 5-9)
- Dual amp control (Big amp + Small PA)

### 💡 LED Display Effects
- **Candy Cane** - Red/white scrolling stripes (default)
- **Christmas Twinkle** - Red/green scrolling stripes
- **Rain Flash Blue** - Pulsing blue warning
- Adjustable speed control

### 🎬 Daily Show Automation
- **Power Up (6:00 PM):**
  - Fans, tablet, and small amp turn on
  - Lid opens (unless raining)
  - Candy cane effect starts
  
- **Power Down (10:45 PM):**
  - Thank you message plays
  - Lid closes
  - All devices shut down except tablet charger

### 🔊 Motion Detection
- PIR sensor triggers random voice greetings
- Sequential playback (tracks 5-9, then loops)
- 3-minute cooldown between triggers
- LED effect changes to "Christmas Twinkle" during playback

## Installation

### ESPHome Configuration

1. **Copy secrets** to your ESPHome `secrets.yaml`:
   ```yaml
   wifi_ssid: "YourSSID"
   wifi_password: "YourPassword"
   ```

2. **Flash Main Controller:**
   ```bash
   esphome run esphome/new-kiosk.yaml
   ```

3. **Flash Timer Display:**
   ```bash
   esphome run esphome/timerkiosk.yaml
   ```

### Home Assistant Setup

1. **Add Package:**
   Copy `homeassistant/kiosk_package.yaml` to your packages directory
   
   Enable packages in `configuration.yaml`:
   ```yaml
   homeassistant:
     packages: !include_dir_named packages
   ```

2. **Import Dashboard:**
   - Open Home Assistant
   - Settings → Dashboards → Add Dashboard
   - Paste contents of `dashboard/kiosk_dashboard.yaml`

### Audio Setup

Create the following MP3 files on the YX5300 SD card (root directory):
- `0001.mp3` - Rain warning message
- `0002.mp3` - Rain clear message  
- `0003.mp3` - Welcome message
- `0004.mp3` - Thank you message
- `0005.mp3` through `0009.mp3` - Motion-triggered greetings

## Wiring

### Main Controller (new-kiosk)

| Device | GPIO | Notes |
|--------|------|-------|
| DHT22 | 4 | Temperature/Humidity |
| PIR Motion | 16 | Motion detection |
| Lid Closed Switch | 17 | INPUT_PULLUP |
| Lid Open Switch | 22 | INPUT_PULLUP |
| Timer Button | 5 | INPUT_PULLUP |
| Kiosk Button | 3 | INPUT_PULLUP |
| Rain DO | 12 | INPUT_PULLUP |
| Rain Analog 1 | 34 | ADC |
| Rain Analog 2 | 36 | ADC |
| VIN Voltage | 35 | ADC (2x divider) |
| WS2812 LED Strip | 13 | 158 LEDs |
| Relay - Lid Open | 19 | Active LOW |
| Relay - Lid Close | 18 | Active LOW |
| Relay - Big Amp | 14 | Active LOW |
| Relay - Fan | 27 | Active LOW |
| Relay - Tablet | 26 | Active LOW |
| Relay - Small Amp | 32 | Active LOW |
| YX5300 TX | 25 | UART to RX |
| YX5300 RX | 33 | UART to TX |
| CYD Display TX | 23 | UART link |
| CYD Display RX | 21 | UART link |

### Timer Display (timerkiosk - CYD)

| Device | GPIO | Notes |
|--------|------|-------|
| TFT CLK | 14 | SPI |
| TFT MOSI | 13 | SPI |
| TFT MISO | 12 | SPI |
| TFT CS | 15 | Chip Select |
| TFT DC | 2 | Data/Command |
| Touch CLK | 25 | SPI |
| Touch MOSI | 32 | SPI |
| Touch MISO | 39 | SPI |
| Touch CS | 33 | Chip Select |
| Touch IRQ | 36 | Interrupt |
| Backlight PWM | 21 | LEDC |
| Main Link TX | 27 | UART to main |
| Main Link RX | 22 | UART to main |

## UART Communication Protocol

The main controller sends these commands to the CYD display:

- `START:XXX\n` - Start timer with XXX seconds
- `RESET:XXX\n` - Reset timer to XXX seconds  
- `STOP\n` - Stop timer and show IDLE

The CYD can send:
- `CMD_START\n` - Request timer start (from touchscreen)

## Configuration

### Timer Duration
Adjust in Home Assistant or via `number.new_kiosk_kiosk_timer_duration_s` (60-900 seconds, default 300)

### LED Speed
Control candy cane scroll speed via `number.new_kiosk_candy_cane_speed` (0-5, default 3)

### Show Hours
Edit automation triggers in `kiosk_package.yaml`:
- Power up: `18:00:00`
- Power down: `22:45:00`

### Actuator Timing
Adjust lid travel time in ESPHome globals:
```yaml
globals:
  - id: actuator_travel_time
    initial_value: "40000"  # milliseconds
```

## Troubleshooting

**Timer won't start:**
- Check UART connection between devices
- Verify `switch.new_kiosk_big_amp_power` entity exists
- Monitor ESPHome logs for "START:" messages

**Rain detection false triggers:**
- Increase `delayed_on` time in rain sensor config
- Check analog sensor readings in dry conditions
- Ensure proper grounding

**LED effects not working:**
- Verify WS2812 data pin connection (GPIO 13)
- Check 5V power supply to LED strip
- Confirm `num_leds: 158` matches your strip

**Lid won't move:**
- Test limit switches (should show in Home Assistant)
- Check relay wiring (active LOW)
- Verify actuator power supply

## Credits

Built by Shawn @ Prickly Guy Creations LLC
GitHub: [@pricklyguy](https://github.com/pricklyguy)

## License

MIT License - Feel free to use and modify for your own projects!
