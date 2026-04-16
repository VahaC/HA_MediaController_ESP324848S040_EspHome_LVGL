# HA_MediaController_ESP324848S040_EspHome_LVGL

# ESP32-4848S040 Media Controller for Music Assistant

A touchscreen media controller built on **ESP32-4848S040** and **ESPHome**, designed to control [Music Assistant](https://music-assistant.io/) players running in Home Assistant.
[Blog with detail explanation](https://vahac.com/esp32-4848s040-media-controller-with-esphome-and-lvgl?utm_source=github))

![ESP32-4848S040](https://img.shields.io/badge/Hardware-ESP32--4848S040-blue)
![ESPHome](https://img.shields.io/badge/ESPHome-2026.3+-green)
![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2024.12+-orange)

---
[Blo]

## Features

- **Playback control** — play/pause, previous/next track
- **Volume control** — volume up/down buttons
- **Now playing** — track title, artist name, album art as background
- **Playback progress** — animated arc showing current position in track
- **Playlist navigation** — dedicated playlists page (4 slots)
- **Cyrillic + Latin + European characters** — full Roboto font with extended glyph set
- **Customizable UI from Home Assistant** — colors and opacity for all UI elements via HA entities
- **Backlight control** — brightness slider directly in HA

---

## Hardware

| Component | Details |
|---|---|
| Board | ESP32-4848S040 |
| MCU | ESP32-S3 dual core 240MHz |
| Display | ST7701S RGB 480×480 |
| Touch | GT911 capacitive |
| Flash | 16MB |
| PSRAM | 8MB octal |

---

## Requirements

### Software
- ESPHome **2026.2+** (tested on 2026.3.3)
- Home Assistant **2024.12+**
- Music Assistant integration for Home Assistant

### Home Assistant setup

**1. Enable ESPHome device actions**

After flashing, go to:
`Settings → Devices & Services → ESPHome → your device → Configure`

Enable **"Allow the device to perform Home Assistant actions"**.

**2. Configure `secrets.yaml`**

```yaml
wifi_ssid: "YourWiFiSSID"
wifi_password: "YourWiFiPassword"
musicassistantesp32s34848s040_encryption_key: "your_api_encryption_key"
musicassistantesp32s34848s040_ota_password: "your_ota_password"
```

---

## Configuration

Edit the `substitutions` block at the top of the YAML file:

```yaml
substitutions:
  device_name: media-controller
  player_entity: media_player.your_player_entity   # Your MA player entity_id
  ha_url: "http://192.168.1.100:8123"              # Your Home Assistant local URL
```

---

## UI Controls available in Home Assistant

After flashing, the following entities appear automatically in HA:

### Color fields (hex string, e.g. `00cfff`)

| Entity | Controls |
|---|---|
| `text.progress_ring_color` | Arc background color |
| `text.progress_ring_fill_color` | Arc indicator (progress) color |
| `text.decoration_color` | Center decoration circle color |
| `text.title_color` | Track title text color |
| `text.artist_color` | Artist name text color |
| `text.volume_color` | Volume label text color |
| `text.buttons_color` | Button border and icon color |

### Opacity sliders (0–255)

| Entity | Controls |
|---|---|
| `number.album_art_opacity` | Album art background opacity |
| `number.progress_ring_opacity` | Progress arc opacity |
| `number.decoration_opacity` | Decoration circle opacity |
| `number.buttons_opacity` | Control buttons opacity |

### Backlight

| Entity | Controls |
|---|---|
| `light.backlight` | Display brightness (dimmer slider) |

---

## Pin mapping

### Display (ST7701S)

| Signal | GPIO |
|---|---|
| CS | 39 |
| DE | 18 |
| HSYNC | 16 |
| VSYNC | 17 |
| PCLK | 21 |
| Red[0:4] | 11, 12, 13, 14, 0 |
| Green[0:5] | 8, 20, 3, 46, 9, 10 |
| Blue[0:4] | 4, 5, 6, 7, 15 |
| SPI CLK | 48 |
| SPI MOSI | 47 |

### Other

| Component | GPIO |
|---|---|
| Backlight | 38 |
| I2C SDA (GT911) | 19 |
| I2C SCL (GT911) | 45 |

---

## Album art

Album art is loaded dynamically from Home Assistant media proxy using the `entity_picture_local` attribute of the media player. The image updates automatically when the track changes.

**Note:** The HA token embedded in the proxy URL may change after HA restart. If album art stops loading, check `entity_picture_local` attribute in Developer Tools and update `ha_url` substitution if needed.

---

## Power supply

Use a stable **5V/2A** power supply. Cheap USB chargers or PC USB ports may cause brownout resets when display brightness changes rapidly. A power bank typically provides better voltage stability.

---

## Known limitations

- Playlist slots (4) must be populated manually — Music Assistant does not expose playlist list via standard media_player attributes
- Album art token in HA proxy URL may change after HA restart
- Cyrillic and special European characters require Roboto font — LVGL built-in Montserrat is used only for control icons (LV_SYMBOL_*)

---

## Project structure

```
musicassistantesp32s34848s040.yaml   # Main ESPHome configuration
secrets.yaml                          # WiFi credentials and API keys (not committed)
```

---

## License

MIT
