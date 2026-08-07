# DCC Signal Decoder (ESP32)

Self-learning DCC accessory decoder for UK colour-light signals (2–4 aspect + route indicator + Position Light), built on an ESP32. Configured entirely through its own built-in web page over WiFi — no PC or Arduino IDE needed after the initial flash.

This is the ESP32 sibling of the original [Arduino Nano-based DCC Signal Decoder](https://github.com/dixieGB/dcc_signal_decoder) — a separate, independent project with its own firmware, its own Uploader tool, and its own release history. **Installers/updates for the two are never interchangeable** — this repo's Uploader only ever checks for updates here, and vice versa.

## What's in this repo

This repo hosts the **firmware** and the **Uploader** — a Windows desktop app used to flash that firmware onto a board.

### Firmware

Runs on an ESP32 Dev Module wired into the signal decoder hardware. Key features:

- **Configured over WiFi** — connect the board to your network (or use it as its own access point) and open its configuration page in any browser: DCC address, signal type, routes/position light role mapping, brightness, Aspect Link/occupancy, and more.
- **2–4 aspect signalling** with a flexible set of general-purpose EX ports, each independently configurable as a Route Indicator, Position Light, or part of a second, fully independent signal head (2/3/4 Aspect) sharing the same board.
- **PWM brightness control**, independently adjustable per LED and per EX port.
- **Aspect Link** — decoders can be chained so an upstream signal automatically reflects the state of the one ahead, for basic automatic block signalling.
- **NVS-backed configuration** (per-field, not a single blob) — every setting survives power loss and firmware updates without wiping unrelated settings.
- **Config export/import** — download a full settings backup as a JSON file, and restore it later or onto another board.
- A JSON-based bench-test API, both over WiFi (the same one the web page uses) and over USB Serial (`SerialApi`) — the same commands work either way.

### DCC Signal Decoder Uploader

A branded Windows `.exe` (Python/Tkinter, packaged with PyInstaller and Inno Setup) that gives end users a simple GUI for:

- **Flashing firmware** — pick a bundled firmware version and a COM port, click Upload. No Arduino IDE, no command line, and the firmware source is never exposed. Everything the board needs — the firmware itself and its web configuration page — is bundled into one flashable image, so a single upload always leaves both in sync.
- **Warns before re-flashing the same version** already running on a connected board.
- **Auto-update** — checks this repo's Releases on startup and offers to download/install a newer version automatically.

Configuring a board today happens through its own web page once it's on WiFi. A future Uploader update will add a "Board Settings" option that opens that same configuration page locally over USB, for boards not yet connected to WiFi.

## Getting the software

Download the latest installer from the [Releases](../../releases/latest) page and run it — it installs the app, an optional CH340 USB-serial driver (needed by most ESP32 Dev Module boards), a Start Menu shortcut, and an uninstaller.
