# The Meshtastic Device Behavior Guide

## What your buttons and LEDs actually do

This guide describes the *standard* behavior baked into the Meshtastic firmware — the button presses, blink patterns, and beeps that work the same way across the large majority of the ~175 supported boards. It's distilled directly from the firmware source (master and develop branches, July 2026), not from documentation, so it reflects what the code does today. Board-specific exceptions are covered at the end; the full per-board breakdown lives in the companion audit spreadsheet.

A quick note on scope: everything here describes the firmware's compile-time defaults. Several behaviors can be changed per-device by the user — the button GPIO can be remapped in device settings, the heartbeat LED can be disabled, and the External Notification module (LED/buzzer/vibration on message) is fully user-configurable. When your device doesn't match this guide, check its settings before suspecting the hardware.

---

## The user button

Almost every Meshtastic device has one primary button (147 of 176 boards). What it does depends on whether your device has a screen.

**On devices with a screen:**

| Press | Action |
|---|---|
| Single click | **Next** — wake the screen, advance to the next page, or move to the next item in an open menu |
| Hold 0.5–4 s | **Select** (activate the highlighted menu item) |
| Hold past ~4 s | Shut down |

Those first two are the entire navigation model on a one-button device: clicking moves, holding commits. Select fires once the hold passes about half a second, so any deliberate press-and-release inside that window is a select; keep holding and you reach the shutdown tier at ~4 seconds.

**On devices without a screen** (trackers, sensor nodes, routers):

| Press | Action |
|---|---|
| Single click | Wake / general user press |
| Double click | Send your position to the mesh ("ping") |
| Triple click | Toggle GPS on or off |
| Hold ~4 s | Shut down |

A few timing details worth knowing: the firmware waits about a quarter second after a click to see if another one is coming (that's the double/triple-click detection window, and why a single click can feel slightly delayed). On devices with a buzzer, holding the button plays a rising "lead-up" chirp starting a little over two seconds in — it's the audible warning that you're approaching the shutdown hold.

## Secondary buttons

Some boards carry more than one button, and each extra button has a fixed role:

**Alt / back button** (25 boards, mostly nRF52-based): single click is an alternate press (typically "next/back" in the UI), and a half-second hold is a long-alt action. On most boards this is whatever the manufacturer labeled as the second button.

**Cancel button** (6 boards, e.g. the Wio Tracker L1 and unPhone): single click cancels / dismisses, and a **4-second hold shuts the device down** — note this is a full second longer than the user button's shutdown hold.

**Capacitive touch pad** (T-Echo family and a few Seeed boards): a half-second touch acts as "back." On e-ink boards with a controllable backlight, the pad is instead a momentary light switch — the backlight is on while your finger is on the pad and goes out when you let go.

**The power button on PMU boards** (T-Beam, T-Beam S3, T-Watch S3): a short click of the actual power button (the one wired to the AXP power-management chip) acts as *cancel* — in practice, it turns the screen off. This is a recent addition; on older firmware the power button click did nothing.

## The status LED: reading the blinks

The single most-asked question — "what is my LED doing?" — has a precise answer. The main status LED runs a small state machine driven by power state:

| Pattern | Meaning |
|---|---|
| Brief blip once per second (mostly off) | Normal operation, running on battery |
| Steady 1-second on/off blink | Charging |
| Solid on | Fully charged (still on USB) |
| Rapid flicker (4×/sec) for ~2 s, repeating every 30 s | **Battery critical — below 5%** |

The once-per-second blip is the "heartbeat," and it's deliberately short (about a millisecond) to save power. If it bothers you, it can be turned off entirely with the *LED heartbeat disabled* device setting — that silences the heartbeat but not the charging indication.

On boards whose LED is wired to the power-management chip or charger IC (T-Beam family, some ThinkNodes), the same states apply but the charge blink may come from the charger hardware itself rather than the firmware.

## Other lights

**Bluetooth pairing LED** (ELECROW ThinkNode family): blinks slowly (2 s cadence) when unpaired, blinks fast (1 s cadence) while pairing, and goes solid once connected. To save power it turns itself off 30 seconds after the last Bluetooth state change — a dark pairing LED doesn't mean disconnected.

**Notification LED** (about 20 boards): lights for one second when a message arrives. This is just the default wiring of the External Notification module, so the duration, nagging, and even which pin it uses are all user-configurable.

**WiFi and BLE indicator LEDs** (a handful of boards, e.g. the Heltec Wireless Bridge): simple connection indicators — on while connected, off otherwise.

**RGB / NeoPixel lighting** (about 22 boards): boards with addressable or I2C RGB LEDs run them through the Ambient Lighting module, which is off by default on most boards and configurable for color and brightness. On some boards the RGB LED doubles as the message-notification light.

## Sounds and vibration

Boards with a buzzer (about 40 of them) get two things by default: system feedback tones (boot, shutdown, the long-press lead-up chirp) and a ringtone when a message arrives, courtesy of the External Notification module with default settings. The ringtone, nag interval, and whether the buzzer sounds at all are user-configurable, and the buzzer can also be remapped or disabled in device settings.

Vibration is rare — as of this audit exactly one board (T-Deck Pro) has a vibration motor wired by default: a half-second buzz per message, renagging every 2 seconds until the notification is acknowledged.

## The notable exceptions

A handful of devices deliberately depart from the standard scheme. If you own one of these, the sections above apply *except* as noted:

**InkHUD e-ink devices** (T-Echo, Heltec Vision Master E213/E290, Wireless Paper, Mesh Pocket, ThinkNode M1, and other e-ink boards running the InkHUD interface — 13 boards): button handling is taken over by the InkHUD UI. Short press advances or selects, long press opens the menu. Where a second button exists it does something board-specific — "next tile" on the Vision Masters; on the T-Echo the touch pad becomes a three-way backlight control (touch = light on while held, hold 5 s = latch the light on, short tap = light off).

**ELECROW ThinkNode M4**: the most LED-endowed device in the fleet — a dedicated heartbeat LED, a Bluetooth pairing LED, and a four-LED battery gauge that lights up for 10 seconds whenever you press a button (1 LED = low, 4 LEDs = above 75%).

**ELECROW ThinkNode M7**: has a dedicated LoRa receive LED that flashes for a tenth of a second every time a packet arrives — a live mesh-traffic monitor. It's also the one device with a factory-reset gesture: on the screenless build, holding the user button for a full **15 seconds** performs a factory reset instead of a shutdown.

**Seeed Mesh Tracker X1**: splits the status LED across two colors — green carries the normal heartbeat and charge patterns, and the critical-battery flicker moves to the red LED.

**T-Echo Card**: replaces discrete status LEDs with single NeoPixels, using color rather than position to distinguish the power and pairing indicators.

**Boards with no LED or no button at all**: roughly 30 boards ship with no firmware-driven LED and about 25 with no button — mostly headless routers, panel devices whose entire interface is a touchscreen (T-Deck, SenseCAP Indicator), and freshly added boards. On those, this guide's screen/app behavior still applies; there's simply nothing to blink or press.

---

*Derived from the Meshtastic firmware device-behavior audit (master + develop, July 2026). For exact pins, per-board define values, and branch differences, see `meshtastic_device_behavior_audit.xlsx` and the accompanying audit report.*
