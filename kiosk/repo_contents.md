# Kiosk Repository Contents

This repository contains everything needed to replicate the 2025 Christmas Kiosk system.

## Repository Structure

```
kiosk/
├── README.md                              # Main documentation
├── SETUP_GUIDE.md                         # Quick start guide
├── REPO_CONTENTS.md                       # This file
├── esphome/
│   ├── new-kiosk.yaml                     # Main ESP32 controller config
│   └── timerkiosk.yaml                    # CYD timer display config
├── homeassistant/
│   └── kiosk_package.yaml                 # Automations & helpers
└── dashboard/
    ├── README.md                          # Dashboard installation notes
    └── kiosk_dashboard_simple.yaml        # Simplified Lovelace dashboard
```

## File Descriptions

### ESPHome Configurations

**new-kiosk.yaml** - Main controller (ESP32-DevKit)
- Manages all sensors, relays, and peripherals
- Controls WS2812 LED strip with 3 effects
- YX5300 MP3 audio playback
- Linear actuator lid control
- UART link to timer display
- Rain detection with automatic shutdown
- Motion-triggered greetings
- ~500 lines of configuration

**timerkiosk.yaml** - Timer display (CYD board)
- 320x240 ILI9341 TFT display
- XPT2046 touchscreen
- Receives timer commands via UART
- Visual countdown with color-coding
- Touchscreen timer start
- ~200 lines of configuration

### Home Assistant Package

**kiosk_package.yaml** - Complete automation package
- 7 automations for full show control
- Rain detection and recovery
- Daily power up/down scheduling  
- Motion-triggered audio
- Display light control
- Input helpers and template sensors

### Dashboard

**kiosk_dashboard_simple.yaml** - Clean Lovelace UI
- Real-time status monitoring
- Timer controls
- Lid operation
- Audio/amp management
- LED effects control
- Voice message buttons
- Tablet control panel
- Environment sensors

## What's NOT Included

- Full xLights show control dashboard (available on request)
- Audio MP3 files (you'll need to create these)
- Fonts for CYD display (use any Roboto font)
- Tablet kiosk configuration (Fully Kiosk Browser)

## Next Steps

1. Read `README.md` for full hardware details
2. Follow `SETUP_GUIDE.md` for installation
3. Customize for your specific needs

## GitHub Repository

This will be published to:
https://github.com/pricklyguy/HomeAssistant/tree/main/kiosk

## License

MIT License - Free to use and modify!

---
**Created by:** Prickly Guy Creations LLC  
**GitHub:** [@pricklyguy](https://github.com/pricklyguy)  
**Year:** 2025
