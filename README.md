# Vidaa TV Legacy - Unofficial Home Assistant Integration Fork

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/integration)

Unofficial fork of the Home Assistant custom component for controlling Hisense / VIDAA Smart TVs. This fork supports Hisense / VIDAA TVs using the legacy RemoteNOW authentication protocol, including transitional models that are not handled well by the original integration. Here, **legacy refers to the authentication protocol, not necessarily the TV's age**.

It communicates directly with the TV's built-in SSL broker — no cloud, no MQTT bridge needed. The original project remains the upstream base, but legacy behavior and fork-specific issues are not supported by the upstream maintainer.

## Features

- **Power on/off** with Wake-on-LAN support
- **Volume control** — up/down/set/mute
- **Input source selection** and **app launching** (Netflix, YouTube, Prime Video, Disney+, etc.)
- **34 remote buttons** as HA button entities (d-pad, playback, channel, numbers, colors)
- **Sensors** — current app and current source
- **Mute switch** — toggle with state awareness
- **SSDP auto-discovery** — TV detected automatically
- **PIN pairing** — secure authentication via TV screen

## Entities

| Platform | Entities | Description |
|----------|----------|-------------|
| Media Player | 1 | Volume, power, source, app launching |
| Remote | 1 | Key sending, activity (app) support |
| Button | 33 | Individual remote keys (18 enabled + 15 disabled by default) |
| Sensor | 2 | Current App, Current Source |
| Switch | 1 | Mute toggle |

Disabled-by-default buttons (enable from device page): color buttons (red/green/yellow/blue), number keys (0-9), subtitle.

## Installation

### HACS (Recommended)

1. Add this repository as a custom repository in HACS
2. Install "Vidaa TV"
3. Restart Home Assistant

### Manual

1. Copy `custom_components/vidaa_tv/` to your HA `config/custom_components/` directory
2. Restart Home Assistant

## Integration branding

Since Home Assistant 2026.3, the icon shown for the installed integration is loaded from `custom_components/vidaa_tv/brand/icon.png` (256 × 256 PNG). The same existing artwork is retained as `icon.png` at the repository root for compatibility with repository listings and tooling. No logo or artificial upscaled `@2x` icon is needed: Home Assistant falls back to the regular icon.

After updating an integration already loaded by Home Assistant, restart Home Assistant to load the updated integration files. TV pairing and configuration do not need to change.

## Setup

1. Go to **Settings** > **Integrations** > **Add Integration** > **Vidaa TV**
2. Enter your TV's IP address (or wait for SSDP auto-discovery)
3. Select the authentication mode:
   - **Legacy Hisense / RemoteNOW**: for TVs using the RemoteNOW-compatible static MQTT/SSL authentication with on-screen PIN pairing, including transitional-generation models such as **Hisense 55A56FEVS**. These TVs do not use modern dynamic VIDAA token authentication. This is the default in this fork.
   - **Modern VIDAA token**: for newer VIDAA TVs compatible with the original token-based flow.
4. Keep **Legacy Hisense / RemoteNOW** for RemoteNOW/PIN-based TVs (including transitional models), regardless of their age. Select **Modern VIDAA token** only when supported by the TV.
5. A PIN will appear on your TV — enter it in the HA UI
6. Done. All entities appear under the device.

No certificates or manual config needed — the library handles authentication automatically.

## Services

| Service | Description |
|---------|-------------|
| `vidaa_tv.send_key` | Send any remote key (e.g. `KEY_HOME`, `KEY_MUTE`) |
| `vidaa_tv.launch_app` | Launch an app by name (e.g. `netflix`, `youtube`) |

## Dashboard Remote Card

A ready-made Lovelace remote card is included in [`dashboard-remote-card.yaml`](dashboard-remote-card.yaml). Uses standard HA cards only (no HACS frontend dependencies):

- Power / Info / Home row
- D-pad (up/down/left/right/OK)
- Back / Menu / Exit
- Channel up/down + mute
- Media player card with volume slider
- Playback controls (rewind/play/pause/stop/ff)
- Current app and source sensors

Copy the YAML into a manual card. Adjust entity IDs to match your TV's name.

## Requirements

- Hisense TV with Vidaa OS
- TV and Home Assistant on the same network
- Home Assistant 2024.1+

## Library

This integration uses [vidaa-control](https://pypi.org/project/vidaa-control/) ([source](https://github.com/tombabolewski/vidaa-control)) — a standalone Python library for Vidaa TV communication.

The original Home Assistant custom component is [ha-vidaa-tv](https://github.com/tombabolewski/ha-vidaa-tv) by Tomasz Babolewski. Use that repository for the upstream project only; fork-specific legacy behavior and bugs should be reported on this fork, not upstream.

## License

MIT
