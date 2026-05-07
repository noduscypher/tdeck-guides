# T-Deck Plus — Battery & Power Guide

## Meshtastic 2.7.x (including MUI / Fancy UI)

> Read this in: [PT](../../pt/technical/battery-energy-guide.md)

---

## 0. Goal

- Extend T-Deck Plus runtime in daily use.
- Use MUI / Fancy UI properly without burning battery for nothing.
- Understand the "weird" behavior of the percentage indicator when you plug in the charger.
- Have a clean workflow for using Bluetooth / the app only when you actually need it.

---

## 1. Hardware and Power Draw — Quick Overview

The biggest power draws on the T-Deck Plus:

- ESP32-S3 (CPU + 2.4 GHz radio)
- Large TFT display
- GPS module
- LoRa radio (especially high-power transmissions)
- Wi-Fi and Bluetooth when active

Core idea:

- Permanently disable anything you don't need.
- Use the screen and Bluetooth only as long as strictly necessary.
- Use the physical side switch as a "manual deep sleep".

---

## 2. Saving Battery in MUI / Fancy UI

### 2.1. MUI Navigation

- **Trackball:**
  - Click = Enter / Select
  - Wheel = navigate through items
- **Touch screen** (if active):
  - Tap = select items / icons
- **Typical menus:**
  - Dashboard / Home
  - Chats / Messages
  - Map
  - Nodes
  - **Settings** (gear icon)
  - Reboot / Shutdown (via Settings or power menu)

---

### 2.2. Display

**Steps:**

1. In MUI, open **Settings** (gear icon).
2. Go to **Display / Screen / UI** (name depends on the build).
3. Adjust:
   - **Brightness:** the lowest comfortable value
   - **Screen On Duration / Screen Timeout / Screen Off:** 15–30 seconds
   - Disable **Always On / Prevent Sleep / Keep Screen On** (if present)
4. Return to the Dashboard.

**Effect:**

- The TFT screen is one of the biggest consumers.
- Less brightness + less time on = a big runtime gain.

---

### 2.3. GPS

**Steps:**

1. In **Settings**, open **Position / Location / GPS**.
2. If you don't need location:
   - Turn off **GPS enabled**.
3. If you need it occasionally:
   - Increase **fix interval / position interval / beacon interval** to **15–30 minutes** or more.
4. Save and go back.

**Effect:**

- Every GPS fix powers up the module and burns a lot of energy.
- Long intervals or GPS off saves substantial battery.

---

### 2.4. Beacon and Telemetry (LoRa)

**Steps:**

1. In **Settings**, open **Position** (or equivalent):
   - Set **Position / Beacon interval** to **15–30 minutes** (or more, if it makes sense).
2. In **Settings**, open **Telemetry**:
   - Set **Telemetry interval** to **10–30 minutes**.
3. Return to the Dashboard.

**Effect:**

- Fewer LoRa transmissions.
- Fewer wakeups for the radio and CPU.
- Lower total consumption.

---

### 2.5. Wi-Fi

If you don't need Wi-Fi:

1. In **Settings**, open **Wi-Fi / Network**.
2. Set:
   - **Wi-Fi = Off / Disabled**
3. Go back.

**Effect:**

- The 2.4 GHz radio (Wi-Fi) stops being awake to scan networks.
- Constant background drain reduced.

---

## 3. Bluetooth with MUI / Fancy UI

### 3.1. The MUI + Bluetooth Concept

- MUI uses the same firmware API as the Bluetooth/Wi-Fi client.
- Result:
  - With MUI running, "normal" Bluetooth for the app isn't always available.
  - To configure via the app, you enter a special mode: **Bluetooth Programming Mode**.
- In that mode:
  - The T-Deck shows a PIN.
  - The app connects.
  - When you exit, you return to normal MUI.

---

### 3.2. Entering Bluetooth Programming Mode

There are two paths (depending on your MUI build):

#### Method 1 — At boot (the "M" logo)

1. Power off the T-Deck via the **side switch**.
2. Flip the switch to power on.
3. When the **Meshtastic logo (M)** appears on screen:
   - Hold your finger down on the logo.
   - Wait until a circle completes (animation).
4. When the circle fills, the device boots into **Bluetooth Programming Mode**.
5. On screen you'll see:
   - A "Programming / Bluetooth Mode" indication.
   - A pairing PIN.
6. On your phone:
   - Open the Meshtastic app.
   - Find the node.
   - Use the PIN shown on the screen to pair.
7. After configuring:
   - Long-press the **Bluetooth icon** on screen (or follow the on-screen instruction) to reboot back into MUI.

#### Method 2 — Via the Reboot/Shutdown menu

1. In MUI, open **Settings**.
2. Go to **Reboot / Shutdown** (or the power menu).
3. On the reboot screen, if multiple icons exist:
   - Power (shutdown)
   - Normal reboot
   - Bluetooth
4. Select the **Bluetooth icon**:
   - The T-Deck reboots into **Bluetooth Programming Mode**.
5. Pair from the app (PIN shown on screen).
6. To return to MUI:
   - Long-press the Bluetooth icon until the device reboots.

**General notes:**

- It's recommended to have **Wi-Fi disabled** before entering Programming Mode to avoid conflicts.
- While you're in Programming Mode, MUI is not running.

---

### 3.3. Battery-Friendly BLE Usage

- **Day to day:**
  - Stay in normal MUI, with no permanent BLE for the app.
- **When you need to change configs from the app:**
  - Disable Wi-Fi in Settings.
  - Enter **Bluetooth Programming Mode** (method 1 or 2).
  - Configure everything in the app.
  - Exit Programming Mode (long-press the Bluetooth icon) to return to MUI.

This way Bluetooth only burns power when you actually need it.

---

## 4. Physical Power Switch

- The T-Deck Plus has a **side switch** that cuts power to the entire board.
- Recommendations:
  - Use the switch to power off when the device will sit idle for hours (overnight, in a backpack, in a bag).
  - In practice this is a "manual deep sleep" with minimal consumption.

---

## 5. Battery Behavior When You Plug In the Charger

### 5.1. Typical Symptom

What many users report on the T-Deck Plus:

- You're at, say, **20%** battery.
- You plug in the USB cable to charge.
- The battery indicator jumps **straight to 100%**.
- It stays at 100% the entire time it's plugged into the charger.
- Often it only goes back to a realistic value after:
  - You unplug the cable.
  - And sometimes only after a **reboot**.

In other words:

- It's **normal not to see the percentage climb** (20 → 21 → 22…).
- Instead it jumps to 100% as soon as it detects "charging".
- The value is unreliable during charging.

---

### 5.2. Why This Happens

- The T-Deck Plus **doesn't have a dedicated fuel gauge**.
- The firmware estimates the percentage based on the **cell voltage measured via ADC**.
- When you plug in the charger:
  - The voltage at the input of the battery line rises significantly.
  - The reading the ESP32 sees is "polluted" / inflated by the charger.
  - The firmware reads something close to 4.2 V or more and assumes "100%".
- Result:
  - Sudden jumps to 100% when you plug in.
  - Sudden drops when you unplug and the voltage settles.
  - "Jumpy", unreliable behavior.

This is a design limitation (hardware + firmware), not a fault in your specific unit.

---

### 5.3. How to Tell If It's Actually Fully Charged

More reliable than the displayed percentage:

1. **Charging LED:**
   - There's a small LED (usually blue) visible through an opening in the case.
   - When the battery is charging: LED solid on.
   - When the battery reaches full charge: LED off.
   - **Rule of thumb:** consider the battery full when the LED goes off, not when the screen shows 100%.

2. **Voltage** (if the firmware shows it):
   - Some debug/telemetry menus expose the voltage in volts.
   - Around **4.20 V** → battery full.
   - 3.7–3.8 V → halfway.
   - 3.5 V or less → close to depleted.

---

### 5.4. Practical Rule of Thumb

- Ignore the "instant 100%" when you plug in the cable.
- Use this flow:
  - Plug the T-Deck into the charger.
  - Leave it charging until the **charging LED goes off**.
  - If you want to confirm, you can reboot after unplugging and check the percentage/voltage at rest.
- In normal use:
  - Start thinking about charging when the real percentage (after some time off-cable) drops below ~30%.

---

## 6. Quick Power & Battery Checklist

### 6.1. Saving Power in MUI (daily use)

- [ ] Brightness low
- [ ] Screen timeout / Screen off at 15–30 s
- [ ] Wi-Fi OFF, except when you actually need it
- [ ] GPS:
  - OFF if you don't need position, or
  - Fix / beacon intervals ≥ 15–30 min
- [ ] Position / Beacon interval ≥ 15 min (if acceptable)
- [ ] Telemetry interval ≥ 10 min
- [ ] Bluetooth:
  - Don't use permanent BLE with MUI
  - Only use Bluetooth Programming Mode to configure via app
- [ ] Physical side switch OFF when you're not using the T-Deck for hours

### 6.2. Charging and Battery Reading

- [ ] Accept that the percentage jumps to 100% when you plug in
- [ ] Use the **charging LED** as the primary "battery full" indicator
- [ ] Optional: confirm voltage in volts if the firmware shows it
- [ ] Don't trust the percentage value during charging
- [ ] After unplugging, if you need a more accurate reading you can:
  - Wait a few minutes at rest, or
  - Do a quick reboot and check the percentage/voltage

---

## 7. Final Notes

- The T-Deck Plus is powerful but not a "smartphone" in terms of battery management.
- A lot of your runtime depends on:
  - How you use the screen.
  - How aggressive your GPS/LoRa intervals are.
  - Keeping Wi-Fi / Bluetooth off most of the time.
- The percentage indicator is approximate only — focus on the charging LED and the voltage (if available).

---

Licensed under CC BY-SA 4.0 · <https://creativecommons.org/licenses/by-sa/4.0/>

*walk quietly, but keep the signal alive.*

₿uilt with Love 🧡 in cooperation with Nature 🌿
