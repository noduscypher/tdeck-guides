> ⚠️ Before you start  
> This guide focuses on using the T‑Deck / T‑Deck Plus with Meshtastic (firmware, UI, maps, battery, troubleshooting).  
> It does not define legal limits or network‑wide best practices.  
> For EU868 presets, roles (CLIENT, ROUTER, etc.), conservative range and regulatory notes in PT/EN/ES, always refer to:  
> https://github.com/noduscypher/mesh-guides

# T-Deck Plus — Touch Fix

## Touchscreen Repair Guide

**Version:** 1.0
**Date:** 2026-04-28

> Read this in: [PT](../../pt/technical/touch-fix-guide.md)

---

## About this document

This document focuses only on fixing touchscreen problems (lag, unresponsiveness, mistapped inputs) on the T-Deck Plus.

Use this guide **only** if your T-Deck Plus's touchscreen is misbehaving after you flashed firmware (Meshtastic, MeshCore, custom UI, etc.).

---

## When do you need TouchFix?

Some T-Deck Plus units ship from the factory with a different touch configuration, and certain firmwares (or touch drivers) can alter the touch chip's internal configuration.

Typical signs you need TouchFix:

- Touchscreen is very laggy or reacts with significant delay.
- Taps are interpreted in the wrong location (e.g., you click one button and a different one activates).
- Touch stops working correctly after flashing Meshtastic, fancy UI, etc.

**TouchFix** is a special firmware that reconfigures the touch chip and corrects these issues.

---

## Process Overview

The full process is:

1. Put the T-Deck Plus into flash mode (DFU).
2. Flash the special file `T-Deck-Plus-TouchFix_241203.bin` using the Espressif web flasher.
3. **Without erasing the chip,** flash the firmware you actually want to use (e.g., Meshtastic via the web flasher).
4. Test touch in the final UI.

> **Important note:** several users report that you should **not do an "erase and install" after running TouchFix** — only "install/update" on top, otherwise you risk wiping the touch correction applied to the chip.

---

## Prerequisites

Before you start, make sure you have:

- **A WebSerial/WebUSB-compatible browser:** Chrome, Edge, or another Chromium-based browser.
- **A USB-C data cable** (not just a charging cable).
- The **TouchFix file:**
  - `T-Deck-Plus-TouchFix_241203.bin` from the `firmware` folder of LilyGO's T-Deck Plus repository.
- The final firmware you want to use:
  - For example, Meshtastic via the Meshtastic Web Flasher (<https://flasher.meshtastic.org>).

Optional, for debugging:

- **SerialTool** (Windows, macOS, Linux) for viewing boot logs and error messages.

---

## How to put the T-Deck Plus into flash mode (DFU)

Putting the T-Deck Plus into flash mode can be a bit temperamental, so follow these steps calmly.

1. Power off the T-Deck Plus completely with the side switch.
2. Make sure the USB cable is connected to the computer, but **not yet** to the T-Deck (or try with the cable already connected to the T-Deck — some users have better luck that way).
3. Hold down the **trackball button**.
4. While holding the trackball, **flip the side switch on**.
5. Release the trackball only after a few seconds.

If it worked:

- The screen has no backlight (looks "off"), but the device is in flash mode.
- The browser should detect a new serial port when you use the web flasher.

If it doesn't work the first time, try:

- Repeating the process with the USB cable already connected to the T-Deck before powering it on.
- The combination: hold **RESET** + trackball, release RESET while still holding the trackball, then power on.

---

## Step 1 — Flash the TouchFix firmware

This step is the same on Mac, Windows, or Linux, as long as you use a compatible browser.

1. Open the **Espressif web flasher:**
   - <https://espressif.github.io/esptool-js/>
2. Click **"Connect"** and pick the port matching the T-Deck Plus.
3. Under **"Add file"**, select `T-Deck-Plus-TouchFix_241203.bin`.
4. Set the write **offset** to `0x0` (zero), **not** `0x10000`.
5. Start the flash and wait until it completes successfully (progress bar and confirmation message).

After this flash:

- The T-Deck may boot without a "pretty" UI, but the touch chip's internal configuration is now corrected.

> **Tip:** if you want to verify everything went well, you can use SerialTool to read the boot log (see "Debugging via SerialTool" section).

---

## Step 2 — Re-flash the Meshtastic firmware (or other)

Now that TouchFix is applied, install the firmware you actually want to use day-to-day (usually Meshtastic).

1. Put the T-Deck Plus back into **flash mode** using the same trackball method.
2. Open the **Meshtastic Web Flasher** in Chrome/Edge:
   - <https://flasher.meshtastic.org>
3. Select the **device** matching the T-Deck Plus from the supported-devices list.
4. Click **"Connect"** and pick the T-Deck's port.
5. Choose the firmware version you want (for example, the Fancy UI firmware for T-Deck Plus, if available).
6. Start the flash by following the web flasher's instructions.

> **Important:** try **not to do a full "Erase"** after running TouchFix — just install the firmware on top, otherwise you can wipe the touch correction applied to the chip.

---

## Step 3 — Test the touchscreen

With the final firmware installed:

1. Power the T-Deck Plus off and back on normally (no trackball).
2. Wait for the firmware to boot completely (Meshtastic UI, etc.).
3. Test touch generally:
   - Tap menu buttons.
   - Scroll, use sliders, on-screen keyboards, etc.

### Practical checks to confirm TouchFix is working

Two quick ways to tell whether the TouchFix update really took:

- **Home button (press and hold)**
  Hold the Home button down and watch how the resync responds: if touch is good, the action responds smoothly and predictably, with no delay or "lost" taps.
- **Switching maps**
  Go to the screen where you can pick different maps and switch between them. If TouchFix is working, you'll clearly notice that taps to switch maps are detected precisely and the change happens without hesitation. It's a quick way to feel the before/after difference.

If everything went well:

- Touch should no longer be laggy.
- Taps should be detected in the right spot, and these Home and map-picker actions become smooth and predictable.

If you're still having problems:

- Check that you used the right TouchFix file for your model.
- Try the LilyGO-recommended UI firmware (e.g., `T-Deck-Plus-Device-UI-v2.6.5.bin`) to test touch without Meshtastic.
- Check recent T-Deck Plus touch documentation and discussions in Meshtastic/LilyGO communities.

---

## Optional — Debugging via SerialTool (Mac / Windows / Linux)

This section is optional and is for when you want to read boot logs, check for touch errors, or confirm the device booted correctly, on any operating system.

### What is SerialTool?

**SerialTool** is a cross-platform serial / TCP / UDP application with a graphical interface, running on Windows, macOS, and Linux.

It's very useful for monitoring T-Deck Plus boot, viewing error messages, and confirming the firmware is alive even when no UI is showing.

### Download and installation

- Official site: <https://serialtool.com> (downloads for Windows, macOS, and Linux).
- On macOS you'll get a `.dmg` or `.zip` containing the app; on Windows, an `.exe` or `.msi` installer; on Linux, a binary or AppImage depending on the release.

### Connecting the T-Deck Plus to SerialTool

1. Connect the T-Deck Plus to the computer via USB-C cable.
2. Open **SerialTool**.
3. In the port list:
   - On Windows, look for something like `COM3`, `COM4`, etc.
   - On macOS, something like `/dev/cu.usbmodem*` or `/dev/cu.SLAB_USBtoUART`.
   - On Linux, ports like `/dev/ttyUSB0` or `/dev/ttyACM0`.
4. Set:
   - **Baud rate:** typically `115200` for ESP32 firmware like Meshtastic.
   - 8N1 (8 data bits, No parity, 1 stop bit), which is the default.
5. Click **Connect** and, if needed, restart the T-Deck to capture the boot log from the start.

### What to look for in the log

While the T-Deck boots, you'll see:

- ESP32 boot messages.
- Firmware output (Meshtastic, UI, etc.), including peripheral initialization errors such as touch.
- If TouchFix was applied correctly, no critical errors should appear when initializing the touch driver.

This is useful for:

- Confirming the device boots even when the screen is black (e.g., brightness at minimum or UI failure).
- Spotting repeated touch-related messages after flashing Meshtastic.

---

Licensed under CC BY-SA 4.0 · <https://creativecommons.org/licenses/by-sa/4.0/>

*walk quietly, but keep the signal alive.*

₿uilt with Love 🧡 in cooperation with Nature 🌿
