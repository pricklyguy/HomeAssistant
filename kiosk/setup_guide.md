# Quick Setup Guide

## 1. Clone or Download Repository

```bash
git clone https://github.com/pricklyguy/HomeAssistant.git
cd HomeAssistant/kiosk
```

## 2. ESPHome Setup

### Add to your `secrets.yaml`:
```yaml
wifi_ssid: "YourNetwork"
wifi_password: "YourPassword"
```

### Flash Devices:
```bash
# Main controller (ESP32-DevKit)
esphome run esphome/new-kiosk.yaml

# Timer display (CYD)
esphome run esphome/timerkiosk.yaml
```

## 3. SD Card Audio Files

Create these MP3 files on your YX5300 SD card:
- `0001.mp3` - Rain warning
- `0002.mp3` - Rain clear
- `0003.mp3` - Welcome
- `0004.mp3` - Thank you
- `0005.mp3` through `0009.mp3` - Motion greetings

## 4. Home Assistant Integration

### Enable Packages
Edit `configuration.yaml`:
```yaml
homeassistant:
  packages: !include_dir_named packages
```

### Install Package
```bash
mkdir -p config/packages
cp homeassistant/kiosk_package.yaml config/packages/
```

Restart Home Assistant.

## 5. Dashboard

1. Settings → Dashboards → "+ Add Dashboard"
2. Name: "Kiosk Control"  
3. ⋮ menu → "Edit Dashboard" → "Raw Configuration Editor"
4. Paste `dashboard/kiosk_dashboard_simple.yaml`
5. Save

## 6. Customize

### Show Hours
Edit automations in `kiosk_package.yaml`:
- Power up time: `18:00:00`
- Power down time: `22:45:00`

### Timer Duration
Change default in Home Assistant or via:
`number.new_kiosk_kiosk_timer_duration_s`

### Actuator Travel Time
In `new-kiosk.yaml`:
```yaml
globals:
  - id: actuator_travel_time
    initial_value: "40000"  # milliseconds
```

## Troubleshooting

**Devices won't connect:**
- Check secrets.yaml credentials
- Verify WiFi signal strength
- Check ESPHome logs

**Timer not working:**
- Verify UART connections (GPIO23/21 on main, GPIO27/22 on CYD)
- Check logs for "START:" messages
- Test with physical button

**Rain sensor false triggers:**
- Increase `delayed_on` filter
- Check sensor wiring and grounding
- Monitor analog values in dry conditions

## Support

Issues? Create a GitHub issue at:
https://github.com/pricklyguy/HomeAssistant/issues
