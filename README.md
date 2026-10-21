# Meshtastic Device Specifications

Hardware design specifications for Meshtastic devices, plus the firmware audit they are built on.

The repository has three layers, and they build on each other:

1. **The audit** — what the firmware source actually does, per board. The evidence base.
2. **The behavior guide** — a plain-language, *descriptive* account of that behavior for users.
3. **The specification series** — ten *prescriptive* documents telling hardware designers what a new Meshtastic device should be.

Everything here derives from the `meshtastic/firmware` source (branches `master` and `develop`, July 2026), not from documentation — so it reflects what the code does rather than what the docs claim. The specifications are all **drafts for discussion** and use MUST / MUST NOT / SHOULD / MAY in the RFC 2119 sense. They are intended for eventual collection into a single unified Meshtastic device design guide.

---

## 1. The audit (evidence base)

| File | Contents |
|---|---|
| [`meshtastic_device_behavior_audit.xlsx`](meshtastic_device_behavior_audit.xlsx) | Per-variant inventory, and the authoritative artifact. Four sheets: **Read Me**, **Develop** (176 variants), **Master** (171 variants), and **Branch Diff**. 30 columns per variant covering buttons, every LED role, polarity, buzzer, vibration, RGB/NeoPixel, touchscreen, and PMU wiring — plus a resolved-behavior column and the raw matched defines. |
| [`audit/`](audit/) | The same data rendered as markdown, readable directly on GitHub: [method and semantics](audit/README.md) plus the branch diff, and one entry per variant for [`develop`](audit/develop.md) and [`master`](audit/master.md). Generated from the spreadsheet by [`convert_xlsx_to_md.py`](audit/convert_xlsx_to_md.py) — re-run it after any change to the .xlsx. |
| [`meshtastic_device_behavior_audit_report.md`](meshtastic_device_behavior_audit_report.md) | The analysis: how a variant define becomes runtime behavior, button and LED semantics traced to the consuming code, an inventory summary, master-vs-develop drift, findings, and method/caveats. |

Scope: LED, button, buzzer, vibration, NeoPixel/RGB and touch defines in every `variants/<platform>/<board>/variant.h`, shared local headers, and `-D` build flags in each board's `platformio.ini` — resolved through the preprocessor chain and cross-referenced against the application code that consumes them. Twelve platform families (esp32, esp32c3/c6/p4/s2/s3, native/portduino, nrf52840, nrf54l15, rp2040, rp2350, stm32).

Notable findings include the T-Beam `PMU_IRQ` reversal on develop, 22 variants declaring LED symbols no firmware code reads, 31 variants with no firmware-driven LED and 24 with no button, and inconsistent `LED_STATE_ON` polarity handling — the bug class that produces an inverted heartbeat.

## 2. The behavior guide (descriptive)

**[`meshtastic_standard_behavior_guide.md`](meshtastic_standard_behavior_guide.md)** — *What your buttons and LEDs actually do.*

The user-facing companion to the audit: the standard button presses, blink patterns, and beeps that work the same way across the large majority of the ~175 supported boards. Covers the user button (with and without a screen), secondary buttons, the status-LED state machine, other indicator LEDs, sounds and vibration, and the notable per-board exceptions (InkHUD e-ink devices, the ELECROW ThinkNode M4/M7, Seeed Mesh Tracker X1, T-Echo Card). Describes compile-time defaults — several behaviors are user-configurable at runtime.

## 3. The specification series (prescriptive)

Ten documents, in series order. Each one closes with a variant-file declaration checklist and a shared rule: **declare exactly what exists** — a define with no hardware, or hardware with no define, fails the audit's dead-define rule.

| # | Document | Prescribes |
|---|---|---|
| 1 | [LED](meshtastic_led_specification.md) | Color assignments (red = status, green = LoRa, blue = Bluetooth, amber = notification); the red-LED-is-GPIO-only rule; drive and polarity; charge status routed through firmware rather than a charger LED; reduced/extended configurations and the four-LED battery gauge; placement and labeling; the NeoPixel alternative; firmware gaps to close. |
| 2 | [Display](meshtastic_display_specification.md) | Three device classes (headless, standard, MUI). The 128×64 OLED floor; e-ink panel sizes and the partial/fast-refresh requirement; MUI-class minimums (color, PSRAM, QVGA+, touch or keyboard); a physical button on every screen device. |
| 3 | [Radio & MCU](meshtastic_radio_mcu_specification.md) | Supported transceivers and MCUs with fleet counts; SX1262 as the default and SX1276 discouraged; original ESP32 prohibited for new designs, S3 and nRF52840 as the standards; minimum flash/RAM per platform; the radio's IRQ, `BUSY` and reset lines on real GPIOs rather than an expander; a selection quick reference. |
| 4 | [Button & Switch](meshtastic_button_specification.md) | A physical user button on every user-facing device; mechanical robustness (the recurring detaching-switch failure); active-low wiring and declaration; the second button's alt vs. cancel roles; standardizing the 15-second factory-reset gesture; a PMU power button rather than a battery cutoff switch, which risks filesystem corruption and gives cancel/screen-sleep for free; an optional GPS switch; making the reset button hard to press by accident; I2C GPIO expanders requiring a wired, wake-capable interrupt line, and the lines that may never sit behind one. |
| 5 | [Power & Battery](meshtastic_power_battery_specification.md) | USB-C that charges from C-to-C (magnetic pogo for sealed designs); JST-PH 2.0 and the Adafruit polarity convention, with reverse-polarity protection required; proper CC/CV charge management; fuel gauge strongly preferred over the analog voltage divider. |
| 6 | [Peripheral Sensor](meshtastic_sensor_specification.md) | Explicitly *not* a limit on what may be included. I2C auto-detection as the integration path; GPS low-power modes that retain ephemeris and a sky-facing patch antenna; IMU/magnetometer axis-orientation documentation; the sealed-case honesty rule for environmental sensors. |
| 7 | [Antenna & RF](meshtastic_antenna_rf_specification.md) | RP-SMA preferred over standard SMA on certification-practice grounds; U.FL as internal-only; never transmit into an unattached antenna; band labeling at the connector and on the antenna; the stock antenna as part of the product; TCXO required (Meshtastic's long packets make drift the failure mode); high-power PA/FEM gain declaration. |
| 8 | [Sound & Haptics](meshtastic_sound_haptics_specification.md) | Passive buzzers only — active buzzers flatten every ringtone and pitch-coded cue; port the sound out of the case; I2S audio as a strict upgrade; vibration recommended for pocket and wearable devices; no sound the firmware cannot silence. |
| 9 | [Recovery & Flashing](meshtastic_recovery_flashing_specification.md) | One prime directive: the device must be essentially unbrickable. A recovery path the payload cannot erase; USB data lines that work; the official web flasher as the acceptance test; recovery gestures reachable without disassembly but not by accident; OTA that falls back rather than going dark; SWD/JTAG pads as the last-resort layer. |
| 10 | [Revisions](meshtastic_revision_specification.md) | Prefer revisions the firmware cannot see; what a firmware-visible revision costs (six product lines already occupy twenty variant entries); keeping the delta to defines rather than a second build; boot-time revision detection, ideally implicit — a re-addressed I2C part is its own marker; marking the revision on the board and never shipping a silent one. |

---

## Where to start

- **Own a Meshtastic device and want to know what it's doing?** Read the [behavior guide](meshtastic_standard_behavior_guide.md).
- **Designing new hardware?** Start with [Radio & MCU](meshtastic_radio_mcu_specification.md) (#3) and [Display](meshtastic_display_specification.md) (#2) — those two decisions set the device class — then work through the rest.
- **Revising a board that already ships?** Read [Revisions](meshtastic_revision_specification.md) (#10) first — it is about keeping a revision from forking the firmware and landing the "which one do I have?" problem on users.
- **Working on the firmware, or checking a specific board?** The [audit report](meshtastic_device_behavior_audit_report.md) explains the define-resolution chain and the consuming code; the [spreadsheet](meshtastic_device_behavior_audit.xlsx) — or [its markdown rendering](audit/) — has the per-variant detail.

Several rules across the series depend on firmware work that hasn't landed yet — the LED spec's §8 and the Button spec's §5 name those items explicitly.
