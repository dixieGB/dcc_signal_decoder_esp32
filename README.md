# DCC Signal Decoder (ESP32)

Self-learning DCC accessory decoder for UK colour-light signals — 2–4 aspect signalling with a flexible set of general-purpose ports for Junction Indicators, Position Lights, and a second independent signal head. Learns its own DCC address from the command station — no PC needed after setup. Configured entirely over USB through a built-in browser-based settings page, no Arduino IDE, no typed commands.

This is the ESP32 sibling of the [Arduino Nano-based DCC Signal Decoder](https://github.com/dixieGB/dcc_signal_decoder_nano) — a separate, independent project with its own firmware and its own Uploader tool. **Firmware and installers between the two are never interchangeable** — this repo's Uploader only ever flashes and updates an ESP32, and only ever checks for updates here. That said, an ESP32-based decoder and a Nano-based decoder work together perfectly well side by side on the same layout — chain any mix of the two via Aspect Link (cascade) for automatic block signalling, board type doesn't matter for that.

## What's in this repo

This repo hosts the **firmware** and the **Uploader** — a Windows desktop app used to flash that firmware onto a board and configure it, with no Arduino IDE or typed commands required.

### Firmware

Runs on an ESP32 Dev Module wired into the signal decoder hardware. Key features:

- **Self-learning address** — hold the board's button to enter Learn Mode, then send the DCC address to teach it from your command station. No PC or source code needed after the initial flash.
- **2–4 aspect signalling** with a flexible set of general-purpose EX ports, each independently configurable as a Junction Indicator, Position Light, or part of a second, fully independent signal head (2/3/4 Aspect) sharing the same board.
- **PWM brightness control**, independently adjustable per LED and per EX port — the primary signal and a second signal head each get their own global dial plus per-LED sliders.
- **Aspect Link** — decoders can be chained so an upstream signal automatically reflects the state of the one ahead, for basic automatic block signalling. Manual overrides are allowed even while a block is occupied (for shunting/manual moves), without ever telling the block behind the line is clear until it actually is.
- **Persistent configuration** (per-field, not a single blob) — every setting survives power loss and firmware updates without wiping unrelated settings.
- **Config export/import** — download a full settings backup as a JSON file, and restore it later or onto another board.
- A JSON-based API over USB Serial, the same one the built-in settings page uses.

### DCC Signal Decoder Uploader

A branded Windows `.exe` (Python/Tkinter, packaged with PyInstaller and Inno Setup) that gives end users a simple GUI for:

- **Flashing firmware** — pick a bundled firmware version and a COM port, click Upload. No Arduino IDE, no command line, and the firmware source is never exposed. Everything the board needs — the firmware itself and its settings page — is bundled into one flashable image, so a single upload always leaves both in sync.
- **Warns before re-flashing the same version** already running on a connected board.
- **Automatic backup and restore** — since every firmware upload wipes the board back to defaults, the Uploader backs up a board's current settings before flashing and restores them automatically once the new firmware is running, no manual re-entry needed.
- **Board Settings** — opens the board's own configuration page locally over USB: DCC address, signal type, Junction Indicator/Position Light/second signal mapping, Aspect Link, brightness, and more, all read from and written to the board live.
- **Serial Monitor** — a raw view of the board's USB serial traffic, useful for diagnostics.
- **User Guides & Change Logs** per firmware version, opened straight from the app.
- **Auto-update** — checks GitHub Releases on startup and offers to download/install a newer version automatically.

## Documentation

The [User Guide](DCC_Signal_Decoder_User_Guide_v1_0.docx) covers everything from installing the Uploader through to configuring the board via Board Settings — the same guide is bundled with the app itself and opens straight from the "User Guide" button next to the firmware version dropdown.

## Getting the software

Download the latest installer from the [Releases](../../releases/latest) page and run it — it installs the app, optional USB-serial drivers (covering most genuine and clone ESP32 Dev Module boards), a Start Menu shortcut, and an uninstaller.
