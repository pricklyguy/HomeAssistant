# Dashboard Installation

The full dashboard YAML is quite large (includes xLights show control). 

To install:

1. Open Home Assistant
2. Go to Settings → Dashboards
3. Click "+ Add Dashboard"
4. Name it "Kiosk Control"
5. Click the ⋮ menu → "Edit Dashboard" → "Raw Configuration Editor"
6. Paste the contents of `kiosk_dashboard.yaml`
7. Save

## Dashboard Preview

The dashboard includes:

- **Kiosk Status Header** - Live health monitoring (battery, temperature, voltage)
- **Kiosk Control Core** - Timer start/stop/reset controls with visual countdown
- **Lid Control** - Dramatic cover control with rain sensor indicators
- **Audio + Amp Power** - Big amp, small PA, and cooling fan toggles
- **Christmas Display Lights** - LED strip control with candy cane speed adjustment  
- **Prickly Guy Voice Buttons** - Quick access to all audio messages
- **Tablet Control Panel** - Browser restart, page reload, device reboot
- **Live Kiosk Camera** - Real-time video feed
- **Environment Sensors** - Temperature, humidity, rain, motion status
- **xLights Show Control** - Full light show integration (optional section)
