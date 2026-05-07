# T-Deck Plus Meshtastic — Complete Guide

**Version 1.0 | April 2026**

> Read this in: [PT](../../pt/technical/complete-guide.md) · [ES](../../es/technical/complete-guide.md) · [JA](../../ja/technical/complete-guide.md)

> 📝 **Note on this version:** the original file lost significant formatting (several tables, code blocks, and ASCII diagrams missing). This version reconstructs the structure in clean Markdown. Where content was completely lost, it's marked with ⚠️ for review before publication. ✓ markers indicate high-confidence reconstructions.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Requirements](#requirements)
3. [Programming Mode (DFU/Flash)](#programming-mode-dfuflash)
4. [Firmware Installation — Erase + Flash](#firmware-installation--erase--flash)
5. [Firmware Update (Normal)](#firmware-update-normal)
6. [Initial Configuration](#initial-configuration)
7. [Bluetooth — Programming Mode](#bluetooth--programming-mode)
8. [Bluetooth — Disable for Standalone Use](#bluetooth--disable-for-standalone-use)
9. [Offline Maps (Optional)](#offline-maps-optional)
10. [Keyboard Shortcuts](#keyboard-shortcuts)
11. [Troubleshooting](#troubleshooting)
12. [Additional Resources](#additional-resources)
13. [Complete Installation Checklist](#complete-installation-checklist)

---

## Introduction

The LilyGO T-Deck Plus is a complete standalone Meshtastic device with screen, QWERTY keyboard, integrated GPS, and LoRa. This guide covers everything from initial installation to advanced configuration.

**Key features:**

- ESP32-S3 + SX1262 LoRa
- Integrated L76K GPS
- 2.8" TFT touchscreen
- Physical QWERTY keyboard
- 18650 battery (not included)
- MicroSD for offline maps

---

## Requirements

### Hardware

- LilyGO T-Deck Plus
- USB-C cable (data, not just charging)
- 18650 battery (recommended: 3000 mAh+)
- LoRa antenna 868 MHz (Europe) or 915 MHz (US)
- MicroSD card FAT32 ≤32 GB (optional, for maps)

### Software

- Chromium-based browser (Chrome, Edge, Brave)
- Active internet connection
- Meshtastic App (Android/iOS) for Bluetooth pairing

### Important Warnings

> ⚠️ **Never power on the device without an antenna connected** — it can damage the LoRa module.
>
> ⚠️ Always use a USB-C cable with data support (not charging-only).
>
> ⚠️ Set the correct region (`EU_868` for Portugal) to comply with ANACOM (Portuguese national communications authority) regulations.

---

## Programming Mode (DFU/Flash)

To flash firmware or perform an erase, the T-Deck Plus needs to enter DFU mode.

### Procedure

1. Power off the T-Deck (side switch OFF).
2. Press and hold the trackball (center button).
3. While holding the trackball, power on the T-Deck (switch ON).
4. Wait 2–3 seconds while keeping the trackball held.
5. Release the trackball.
6. Connect the USB-C cable to your computer.

### Confirming DFU Mode

- The T-Deck screen goes completely black (no backlight).
- The computer detects a device: **"USB JTAG/serial debug unit"** or **"ESP32-S3"**.

> **Note:** if it doesn't work the first time, repeat the process. Some USB-C to USB-C cables can cause problems — try a USB-A to USB-C cable instead.

---

## Firmware Installation — Erase + Flash

**When to use:** first installation, device with serious problems, switching firmware type (e.g., alpha → beta).

### Step by Step

1. **Enter Programming Mode**
   Follow the procedure from the previous section.

2. **Open the Web Flasher**
   - Open a Chromium-based browser
   - Go to: <https://flasher.meshtastic.org>

3. **Select the Device**
   - Click "Select Target Device"
   - Choose: **"LILYGO T-Deck"** (works for T-Deck Plus)

4. **Choose Firmware Version**
   - **Recommended:** select the latest BETA version (avoid alpha in production)
   - Don't use alpha versions for daily use

5. **Configure Full Erase**
   - ✅ Enable the option: **"Full Erase and Install"**
   - This wipes all flash memory, including old configurations

6. **Connect the Device**
   - Click "Connect"
   - Select port: "USB JTAG/serial debug unit"
   - Click "Connect"

7. **Flash**
   - Click "Flash" then "CONTINUE"
   - Wait for completion (1–3 minutes)
   - **DO NOT disconnect USB during the process**

8. **Finalization**
   - When you see "Flash Complete", close the browser
   - Power off the T-Deck (switch OFF)
   - Power on again (switch ON)
   - The device boots with fresh firmware and MUI (Meshtastic UI)

---

## Firmware Update (Normal)

**When to use:** updating to the latest version without losing configurations.

### Step by Step

1. **Enter Programming Mode**
   Follow the procedure: power off → hold trackball → power on → wait → release → connect USB.

2. **Web Flasher**
   - Go to: <https://flasher.meshtastic.org>
   - Select: "LILYGO T-Deck"
   - Choose the desired firmware version

3. **Update (Without Erase)**
   - ❌ **Disable** the "Full Erase" option
   - Click "Flash"

4. **Flash and Reboot**
   - Wait for completion
   - Power off and on the T-Deck
   - Configurations are preserved (region, node name, channels, etc.)

> **Note:** if you have problems after the update, do a Full Erase + Flash.

---

## Initial Configuration

After the initial flash, configure the essential parameters.

### Via Meshtastic Web Client

1. **Connect via USB Serial**
   - Go to: <https://client.meshtastic.org>
   - Click "Connect" → "USB Serial"
   - Select the T-Deck's port

2. **Set LoRa Region**
   - Side menu: **"LoRa"**
   - Region: select **EU_868** (Portugal/Europe)
   - Click "Save"

   > ⚠️ **Mandatory in Portugal:** EU_868 (868 MHz, 500 mW, 10% duty cycle), per ANACOM regulations.

3. **Configure GPS Position**
   - Menu: **"Position"**
   - GPS Mode: "Enabled"
   - GPS Update Interval: 120 s (adjust as needed)
   - Position Broadcast: enable if you want to share your location
   - Click "Save"

4. **Node Info (Optional)**
   - Menu: **"Settings" → "Device"**
   - Node Name: set your node's name
   - Role: CLIENT (normal use) or ROUTER (fixed node)

5. **Channels (Optional)**
   - Menu: **"Channels"**
   - Configure additional channels or join an existing mesh
   - The Primary Channel comes pre-configured

### Via MUI on the T-Deck

After initial configuration via web/app, you can adjust on the T-Deck itself:

- **Settings → LoRa** → check the region
- **Settings → Position** → GPS status
- **Map** → view nodes and GPS position

---

## Bluetooth — Programming Mode

**When to use:** initial pairing with a smartphone, configuration via app, troubleshooting.

### Activating Bluetooth Programming Mode

#### Via MUI (Firmware 2.6+)

1. On the T-Deck, go to: **Settings** (gear icon)
2. Select: **"Reboot/Shutdown"**
3. Click the Bluetooth icon (center of the screen)
4. The screen shows: **">> Programming Mode <<"** with a passcode (e.g., 123456)

#### Pairing with a Smartphone

1. Open the Meshtastic App (Android/iOS)
2. Go to Bluetooth → Scan
3. Select your T-Deck (node name)
4. Enter the passcode shown on the T-Deck's screen
5. Connect and configure via the app

### Exiting Bluetooth Programming Mode

1. Long-press the Bluetooth icon on the T-Deck's screen
2. The device reboots back to normal MUI

> **Important:**
> - Bluetooth Programming Mode is for initial pairing/configuration.
> - For daily Bluetooth use, switch to BaseUI (next section).
> - MUI doesn't support permanently active Bluetooth (it uses the same API).

---

## Bluetooth — Disable for Standalone Use

**When to use:** standalone operation without a smartphone, saving battery, using MUI with maps.

### Option 1: Use MUI (Recommended Standalone Mode)

MUI (Meshtastic UI) is the full visual interface, but doesn't support simultaneous Bluetooth.

- **Advantages:** maps, keyboard, full mesh visualization
- **Disadvantage:** no Bluetooth for the app

To make sure Bluetooth is off:

**Via Meshtastic CLI:**

```bash
meshtastic --set bluetooth.enabled false
```

> ✓ Reconstructed: `meshtastic --set bluetooth.enabled false` (standard CLI syntax, confident)

**Or via Web Client:**

- Settings → Bluetooth → Enabled: OFF → Save

### Option 2: BaseUI (Permanent Bluetooth)

If you need Bluetooth active for the app 24/7:

#### Switching to BaseUI

**Via CLI:**

> ⚠️ **[VERIFY]** CLI command missing from source. Confirm the correct syntax against current Meshtastic documentation — the configuration key for switching to BaseUI may vary between versions. Alternatively, use the Web Client path below.

**Or via Web Client:**

- Settings → Device → Display Type: BaseUI
- Save and reboot

**BaseUI characteristics:**

- Simple interface (no visual maps)
- Bluetooth works permanently
- Lower resource consumption
- Ideal for control via app

### Quick Comparison

> ⚠️ Table reconstructed from the document's own descriptive context. Verify against official documentation before publication.

| Feature | MUI | BaseUI |
|---|---|---|
| Visual maps | ✅ | ❌ |
| Full keyboard & navigation | ✅ | Partial |
| Permanent Bluetooth for app | ❌ (Programming Mode only) | ✅ |
| Resource consumption | Higher | Lower |
| Use case | Standalone with maps | App-driven control |

> **Recommendation:** use MUI for standalone with maps. Switch to BaseUI if you need the smartphone app permanently connected.

---

## Offline Maps (Optional)

Adding offline maps to the T-Deck Plus lets you visualize the mesh network over real cartography.

### Requirements

- MicroSD card FAT32, ≤32 GB
- Map tile pack (OpenStreetMap, Google Maps, Atlas)

### Correct Folder Structure

The tile structure follows the standard XYZ format used by OpenStreetMap:

```
/maps/
└── <pack-name>/
    ├── 0/                  ← zoom level 0 (entire world)
    ├── 1/
    ├── 2/
    ├── ...
    ├── 8/                  ← essential zoom levels for Portugal
    ├── 9/
    ├── 10/
    ├── 11/
    ├── 12/                 ← high zoom, many tiles
    ├── 13/
    ├── 14/
    └── 15/                 ← maximum zoom
```

**Important:**

- Each numbered folder (0–15) corresponds to a zoom level.
- Inside each zoom folder there are `X/` subfolders and `Y.png` files (XYZ format).
- Low zoom levels (0–5) cover the entire world with few tiles.
- High zoom levels (8–15) contain many tiles and are essential for local detail.
- For Portugal, zoom levels 8–14 are what really matters.
- Zoom levels 1–5 can be empty without issue.

### Step-by-Step Installation

#### Method 1: Atlas Pre-Built Pack (Recommended Global)

1. **Download the map pack:**
   - Atlas (global, includes Europe): <https://github.com/meshtastic/device-ui/tree/master/maps>
   - File: `atlas.zip` ⚠️ *[exact filename to verify in the repo]*

2. **Extract the ZIP:**
   - Unzip `atlas.zip`
   - You'll get an `atlas/` folder with subfolders `0/` to `15/`

3. **Copy to the SD card:**
   - Create the `/maps/` folder at the root of the SD card
   - Copy the entire `atlas/` folder to `/maps/atlas/`
   - Final structure: `/maps/atlas/[0-15]/...`

4. **Verify the contents:**
   - Open `/maps/atlas/12/` (or another high zoom level)
   - It should contain subfolders and `.png` files
   - If empty, the pack is corrupted

#### Method 2: Portugal Pack (mapsportugal115OSM.zip)

To use the Portugal-specific pack:

1. **Extract the ZIP file:**
   - Unzip `mapsportugal115OSM.zip`
   - You'll get a folder structure

2. **Navigate to the `openstreetmap/` folder** inside the extracted ZIP.

3. **Copy the contents of the `openstreetmap/` folder:**
   - Enter the `openstreetmap/` folder from the extracted ZIP
   - Select ALL the numbered folders (1, 2, 3... up to 15)
   - Copy (Ctrl+C / Cmd+C)

4. **Paste into the SD inside the "Google Maps" folder:**
   - Insert the SD card into the computer
   - Navigate to: `/maps/Google Maps/` on the SD card
   - Paste (Ctrl+V / Cmd+V) all the copied content

5. **Final structure on the SD:**

   ```
   /maps/
   └── Google Maps/
       ├── 1/
       ├── 2/
       ├── ...
       └── 15/
   ```

6. **Verify the installation:**
   - Open `/maps/Google Maps/12/` (or another high zoom level) on the SD
   - It should contain numbered subfolders and files
   - If empty, repeat the extraction and copy

7. **Use it on the T-Deck:**
   - Insert the SD into the T-Deck
   - Go to Map
   - Long-press the map icon → select "Google Maps"
   - Zoom to Portugal and wait for GPS fix

> **Note:** the "Google Maps" folder name on the SD is just a label — you can rename it to "Portugal" or anything else if you prefer. What matters is the internal structure (folders 1–15 with tiles).

#### Method 3: Generate Custom Tiles

**MapDL Python script:**

```bash
# Download the script
wget https://gist.githubusercontent.com/droberin/b333a216d860361e329e74f59f4af4ba/raw/MapDL.py

# Edit coordinates for Portugal
# lat: 37 to 42
# lon: -9.5 to -6
# zoom: 8-14

python3 MapDL.py

# Copy output to /maps/portugal/
```

**MOBAC (Mobile Atlas Creator):**

- Download: <https://mobac.sourceforge.io>
- Select region: Portugal
- Zoom levels: 8–14
- Output format: OSM Tile format
- Export and copy to the SD

### Using Maps on the T-Deck

1. Insert the SD card into the T-Deck (side slot).
2. Power on the device.
3. Go to Map (map icon).
4. Long-press the map icon (or Settings → Map).
5. Select the source: `atlas`, `Google Maps`, `portugal`, or whatever name you used.
6. Zoom in to Portugal (trackball + scroll).
7. Get GPS fix outdoors — your position appears on the map.

### Maps Troubleshooting

**Map doesn't appear:**

- Is the SD formatted as FAT32? (not NTFS/exFAT)
- Is the structure `/maps/<name>/[0-15]/` correct?
- Do zoom folders 8–14 contain `.png` files?

**Map loads slowly:**

- Slow SD card (class 10 or higher recommended).
- Too many zoom levels (delete 1–7 if you don't use them).

**Wrong coordinates:**

- Wait for GPS fix (initial cold start can take 5–10 min).
- Go outside with a clear sky view.

---

## Keyboard Shortcuts

> ⚠️ **[ORIGINAL CONTENT LOST — verify before publication]**
>
> The complete T-Deck Plus keyboard shortcuts table couldn't be recovered from the damaged source. We suggest restoring it from one of these sources:
>
> - Official device-ui repository: <https://github.com/meshtastic/device-ui>
> - Official Meshtastic docs: <https://meshtastic.org/docs>
> - Meshtastic Portugal community discussions: <https://meshtastic.pt>

### General Notes (recovered from source)

- **ALT + key / SYM + key:** press and release (no need to hold).
- `SYM + key` shortcuts are not active in the SND and SET tabs.
- The **Fn** key may need activation in firmware 2.7+.

---

## Troubleshooting

### Device Won't Enter DFU Mode

**Symptoms:** screen powers on normally, computer doesn't detect a flash device.

**Solutions:**

1. Repeat the sequence: power off → hold trackball → power on → wait 3 s → release.
2. Test the cable: use USB-A to USB-C (instead of C-C).
3. Drivers: Windows may need CH340/CP210x drivers.
4. Try 3–5 times: timing is critical.

### Bluetooth Won't Connect

**Symptoms:** the app doesn't see the device, pairing fails.

**Solutions:**

**If in MUI:**

- Bluetooth doesn't work in normal MUI.
- Use Bluetooth Programming Mode: Settings → Reboot → Bluetooth icon.
- Or switch to BaseUI permanently.

**If Bluetooth is disabled:**

```bash
meshtastic --set bluetooth.enabled true
```

> ✓ Reconstructed: `meshtastic --set bluetooth.enabled true` (standard CLI syntax, confident)

**If after a firmware update:**

- It may disable itself.
- Re-enable via CLI or web client.
- If it persists, roll back to a previous firmware (2.5.18 known to work).

**iPhone issues:**

- Some firmwares (early 2.6.x) have iOS bugs.
- Use the web client over USB as an alternative.
- Update to the latest beta firmware.

### GPS Won't Fix

**Symptoms:** position doesn't update, "No GPS fix" persists.

**Solutions:**

1. Go outside: GPS needs a clear sky view.
2. Wait 5–15 min: first fix takes time (cold start).
3. Check the configuration:

   ```bash
   meshtastic --get position
   ```

   > ✓ Reconstructed: `meshtastic --get position` (standard CLI syntax, confident)

4. Correct GPIO pins (T-Deck Plus):
   - RX: GPIO 34
   - TX: GPIO 12
   - Configure via web client: Position → GPS Pins

5. GPS antenna: some models have an integrated ceramic antenna, others need an external one.

### Maps Don't Appear

See the [Maps Troubleshooting](#maps-troubleshooting) section under [Offline Maps](#offline-maps-optional).

### Device Won't Power On

**Solutions:**

1. **Battery:** check that the 18650 is inserted correctly (+/- poles).
2. **Charging:** connect USB-C, wait 30 min.
3. **Side switch:** confirm it's in the ON position.
4. **Reset:** press the RESET button (small hole on the side).

### Firmware Brick / Won't Boot

**Solutions:**

1. **Full Erase + Flash:** DFU Mode → Full Erase → Flash beta firmware.
2. **Try different firmwares:** 2.5.x vs 2.6.x.
3. **Meshtastic CLI noproto:**

   ```bash
   meshtastic --noproto
   ```

   > ✓ Reconstructed: `meshtastic --noproto` (standard CLI syntax, confident)

4. **Factory reset via CLI:**

   ```bash
   meshtastic --factory-reset
   ```

   > ✓ Reconstructed: `meshtastic --factory-reset` (standard CLI syntax, confident)

### Nodes Don't Appear / Empty Mesh

**Solutions:**

1. **Correct region:** EU_868 in Portugal.
2. **Antenna connected:** no antenna = no TX/RX.
3. **Frequency slot:** check whether the local mesh uses a different slot.
4. **Range:** nodes need to be within LoRa range (~5 km urban, 20 km+ rural).
5. **Channels:** the Primary Channel must match the local mesh.

### High Battery Drain

**Solutions:**

1. **Disable Bluetooth** if not in use.
2. **GPS update interval:** increase to 300 s+ (instead of 120 s).
3. **Screen timeout:** enable timeout so the screen turns off.
4. **Role:** set as CLIENT (not ROUTER) if mobile.

> For full details, see [battery-energy-guide.md](battery-energy-guide.md).

---

## Additional Resources

### Official Links

- **Meshtastic Web Flasher:** <https://flasher.meshtastic.org>
- **Meshtastic Web Client:** <https://client.meshtastic.org>
- **Official Documentation:** <https://meshtastic.org/docs>
- **Firmware Releases:** <https://github.com/meshtastic/firmware/releases>

### Portugal Community

- **Meshtastic PT:** <https://meshtastic.pt>
- **Portugal Nodes Map:** <https://meshtastic.pt/map>
- **Malha PT:** <https://malha.meshtastic.pt>
- **PT FAQ:** <https://meshtastic.pt/faq>
- **Facebook Meshtastic Portugal:** active group with support

### Maps Resources

- **MUI Maps GitHub:** <https://github.com/meshtastic/device-ui/tree/master/maps>
- **MapDL Script:** <https://gist.github.com/droberin/b333a216d860361e329e74f59f4af4ba>
- **MOBAC:** <https://mobac.sourceforge.io>
- **Wind & Wireless Guide:** <https://windandwireless.com/pages/techlab/meshtasticmaps>

### Python CLI

```bash
# Install CLI
pip3 install --upgrade "meshtastic[cli]"

# Useful commands
meshtastic --info                              # Node info
meshtastic --nodes                             # List mesh nodes
meshtastic --set lora.region EU_868            # Set region
meshtastic --set bluetooth.enabled false       # Disable Bluetooth
meshtastic --get device                        # Full configuration
meshtastic --factory-reset                     # Total reset
```

### Portugal Regulation (ANACOM)

- **Frequency:** 868 MHz (EU_868)
- **Maximum power:** 500 mW (27 dBm)
- **Duty cycle:** 10%
- **Alternative:** 433 MHz, 10 mW, 10% duty cycle

---

## Complete Installation Checklist

- [ ] LoRa antenna connected
- [ ] 18650 battery inserted
- [ ] Firmware flashed (beta recommended)
- [ ] LoRa region: EU_868
- [ ] GPS enabled and pins correct
- [ ] Node name configured
- [ ] Bluetooth: Programming Mode tested + disabled (if standalone)
- [ ] Offline maps installed (optional)
- [ ] GPS fix obtained (outdoors)
- [ ] First mesh node detected
- [ ] Smartphone app paired (if using Bluetooth)

---

## Guide Changelog

- **v1.0 (April 2026):** initial complete release.

---

> **Contributions:** this guide is open-source. Improvements and corrections welcome via the Meshtastic PT community.

---

☥ cypherpunks write code /// nature writes resilience /// we bridge both ☥

Licensed under CC BY-SA 4.0 · <https://creativecommons.org/licenses/by-sa/4.0/>

*walk quietly, but keep the signal alive.*

₿uilt with Love 🧡 in cooperation with Nature 🌿
