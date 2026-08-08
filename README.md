# DCC Signal Decoder (ESP32)

Self-learning DCC accessory decoder for UK colour-light signals (2–4 aspect + route indicator + Position Light), built on an ESP32. Configured entirely through its own built-in web page, either over WiFi or (with no WiFi needed at all) over USB via the desktop Uploader's Board Settings bridge.

This is the ESP32 sibling of the original [Arduino Nano-based DCC Signal Decoder](https://github.com/dixieGB/dcc_signal_decoder_nano) — a separate, independent project with its own firmware, its own Uploader tool, and its own release history. **Installers/updates for the two are never interchangeable** — this repo's Uploader only ever checks for updates here, and vice versa.

## What's in this repo

This repo hosts the **firmware** and the **Uploader** — a Windows desktop app used to flash that firmware onto a board.

### Firmware

Runs on an ESP32 Dev Module wired into the signal decoder hardware. Key features:

- **Configured over WiFi** — connect the board to your network (or use it as its own access point) and open its configuration page in any browser: DCC address, signal type, routes/position light role mapping, brightness, Aspect Link/occupancy, and more.
- **2–4 aspect signalling** with a flexible set of general-purpose EX ports, each independently configurable as a Route Indicator, Position Light, or part of a second, fully independent signal head (2/3/4 Aspect) sharing the same board.
- **PWM brightness control**, independently adjustable per LED and per EX port — the primary signal and a second signal head each get their own global dial plus per-LED sliders.
- **Aspect Link** — decoders can be chained so an upstream signal automatically reflects the state of the one ahead, for basic automatic block signalling. Manual overrides are allowed even while a block is occupied (for shunting/manual moves), without ever telling the block behind the line is clear until it actually is.
- **NVS-backed configuration** (per-field, not a single blob) — every setting survives power loss and firmware updates without wiping unrelated settings.
- **Config export/import** — download a full settings backup as a JSON file, and restore it later or onto another board.
- **Best-effort update check** — the board itself can check this repo for a newer firmware release (when it has real internet access) and shows a dismissible notice on its own web page.
- A JSON-based bench-test API, both over WiFi (the same one the web page uses) and over USB Serial (`SerialApi`) — the same commands work either way.

### DCC Signal Decoder Uploader

A branded Windows `.exe` (Python/Tkinter, packaged with PyInstaller and Inno Setup) that gives end users a simple GUI for:

- **Flashing firmware** — pick a bundled firmware version and a COM port, click Upload. No Arduino IDE, no command line, and the firmware source is never exposed. Everything the board needs — the firmware itself and its web configuration page — is bundled into one flashable image, so a single upload always leaves both in sync.
- **Warns before re-flashing the same version** already running on a connected board.
- **Automatic backup and restore** — since every firmware upload wipes the board back to defaults, the Uploader can back up a board's current settings before flashing and restore them automatically once the new firmware is running, no WiFi or manual re-entry needed.
- **Board Settings over USB** — opens the board's own configuration page locally, reached over the USB connection instead of WiFi, for boards not yet connected to a network.
- **Serial Monitor** — a raw view of the board's USB serial traffic, useful for diagnostics.
- **Auto-update** — checks this repo's Releases on startup and offers to download/install a newer version automatically.

## Getting the software

Download the latest installer from the [Releases](../../releases/latest) page and run it — it installs the app, an optional CH340 USB-serial driver (needed by most ESP32 Dev Module boards), a Start Menu shortcut, and an uninstaller.
