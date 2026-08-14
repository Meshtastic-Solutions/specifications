# Meshtastic Device Behavior Audit — Variant Defines, LEDs, and Buttons

**Source of truth:** `meshtastic/firmware` source code, branches `master` and `develop` (snapshots taken 2026-07-13). **Scope:** LED, button, buzzer, vibration, NeoPixel/RGB, and touch-related defines in every `variants/<platform>/<board>/variant.h`, locally included shared headers, and `-D` build flags in each board's `platformio.ini`, cross-referenced against the consuming application code that gives those defines their runtime behavior. **Companion:** `meshtastic_device_behavior_audit.xlsx` carries the full per-variant inventory (one row per variant, both branches, plus a branch-diff sheet); this report covers the behavioral analysis, branch drift, and findings.

The audit covers 176 variants on develop and 171 on master, across twelve platform families (esp32, esp32c3, esp32c6, esp32p4, esp32s2, esp32s3, native/portduino, nrf52840, nrf54l15, rp2040, rp2350, stm32).

## 1. How a variant define becomes device behavior

A variant's raw defines pass through a resolution chain before any code consumes them, and reading the variant file alone can be misleading. The chain is: `variant.h` (plus any `-D` flags in the board's `platformio.ini`) → the platform's `architecture.h` → `configuration.h` defaults → consuming code. The mappings that matter for this audit: on nRF52, `PIN_LED1` becomes `LED_POWER` when the variant hasn't set one explicitly, `PIN_BUTTON1` becomes `BUTTON_PIN`, and `PIN_BUTTON_TOUCH` becomes `BUTTON_PIN_TOUCH` (the last also on ESP32). In `configuration.h`, `PIN_BUTTON2` becomes `ALT_BUTTON_PIN` unless the variant already claims a `CANCEL_BUTTON_PIN`, `LED_STATE_ON` defaults to 1 (active-high), and `BUTTON_ACTIVE_LOW`/`BUTTON_ACTIVE_PULLUP` default to true. On ESP32, `HAS_AXP192`/`HAS_AXP2101` derive `HAS_PMU`, which puts the PMU's charge LED under firmware control. The spreadsheet's "effective" columns are post-chain values; the raw column preserves what the variant literally says.

### Button semantics (src/input/InputBroker.cpp + ButtonThread)

All GPIO buttons are debounced OneButton instances feeding the InputBroker event bus; which events fire is fixed per button role at compile time, and the user can remap the primary button's GPIO at runtime (`device.button_gpio`).

The **user button** (`BUTTON_PIN`) behaves differently depending on whether the build has a screen. With a screen: single click emits `USER_PRESS` (advances/wakes the UI), a 500 ms hold emits `SELECT` (menu select), and a 3.9 s hold emits `SHUTDOWN`. Without a screen: single click `USER_PRESS`, double click `SEND_PING` (broadcast position), triple click `GPS_TOGGLE`, 3.9 s hold `SHUTDOWN`. The ThinkNode M7 gets a hard-coded exception in InputBroker: its screenless long-long press is stretched to 15 s and fires `FACTORY_RESET` instead. A hold past 2.2 s plays a lead-up sound on buzzer-equipped boards.

The **alt/back button** (`ALT_BUTTON_PIN`, usually auto-mapped from `PIN_BUTTON2`) emits `ALT_PRESS` on click and `ALT_LONG` at 500 ms. The **cancel button** (`CANCEL_BUTTON_PIN`) emits `CANCEL` on click and `SHUTDOWN` at a 4 s hold. The **touch button** (`BUTTON_PIN_TOUCH`, capacitive pad on T-Echo-family hardware) normally emits `BACK` at 500 ms, but on boards with an e-ink backlight rail (`PIN_EINK_EN`) it is rewired as a momentary backlight: backlight on while touched, off on release, no broker event at all. Boards with `HAPTIC_FEEDBACK_PIN` add a haptic blip on touch and a second at the long-press threshold. Three PMU boards (`PMU_POWER_BUTTON_IS_CANCEL`) map a short click of the AXP power-management chip's own button to `CANCEL`, i.e. screen off.

InkHUD e-ink builds are a separate regime: when the InkHUD display mode is active, button handling moves from ButtonThread to the niche `TwoButton` driver, wired per-variant in `nicheGraphics.h`. Button 0 is the user button (short press advances/selects in InkHUD, long press opens the menu); button 1 varies — an aux "next tile" button on the Vision Master boards, the capacitive pad driving backlight peek/latch/off on the T-Echo. Thirteen variants on develop ship InkHUD wiring; the spreadsheet's behavior column carries each one's exact mapping.

### LED semantics (src/modules/StatusLEDModule.cpp and friends)

`LED_POWER` is the general status LED, run by StatusLEDModule on a 1 s tick: discharging shows a heartbeat (a ~1 ms blip once per second), charging toggles at 1 s, fully charged goes solid, and below 5% battery the LED does a 2 s burst of 250 ms blinks every 30 s. The whole behavior honors the user's `led_heartbeat_disabled` config. `LED_POWER_CRITICAL` splits the critical pattern onto a second LED when it's a distinct pin. `LED_HEARTBEAT` moves the heartbeat blip to a dedicated LED and forces the charge LED off while discharging. `LED_PAIRING` tracks Bluetooth: 2 s blink unpaired, 1 s blink while pairing, solid when connected, auto-off 30 s after the last state change. `POWER_LED_HARDWARE_BLINKS_WHILE_CHARGING` tells firmware not to toggle during charge because the board's charger IC blinks the LED itself. On PMU boards the same state machine drives the AXP chip's charge LED; on the ThinkNode M5 it drives LEDs behind a PCA I/O expander (`PCA_LED_*`); on the T-Echo Card it drives single NeoPixels with per-function colors (`NEOPIXEL_STATUS_*`).

`LED_NOTIFICATION` is consumed by NodeDB's default-config logic, which enables the External Notification module with that pin as output (1000 ms on message, `LED_STATE_ON` polarity). The same defaults wire `PIN_BUZZER` (PWM ringtone, alert on message) and `PIN_VIBRATION` (500 ms pulse, 2 s nag). `LED_LORA` (ThinkNode M7 only) flashes 100 ms per received LoRa packet. `WIFI_LED` and `BLE_LED` are simple connection indicators. `Battery_LED_1..4` (ThinkNode M4 only) is a four-step charge gauge at 25/50/75% thresholds, shown for 10 s after a button press or while charging. Ambient RGB hardware — NeoPixel strips (`HAS_NEOPIXEL`), NCP5623 and LP5562 I2C drivers, discrete `RGBLED_*` pins — is run by the AmbientLighting module and doubles as a notification/status surface on some boards.

## 2. Inventory summary (develop, effective values)

| Feature | Variants | Notes |
|---|---|---|
| User button (`BUTTON_PIN`) | 147 of 176 | 24 variants have no button at all — headless bridges/routers (rak11200, heltec_wireless_bridge, the stm32 targets), touch/trackball-driven panels (t-deck, elecrow_panel, m5stack_cores3), portduino, and new bring-up targets |
| Alt/back button | 25 | Mostly nRF52 boards via `PIN_BUTTON2` auto-mapping |
| Cancel button | 6 | link32_s3, unPhone, meshtiny, muzi_base, Wio Tracker L1 (+eink) |
| Capacitive touch button | 4 | seeed_solar_node, t-echo, t-echo-plus, t-impulse-plus |
| Power/status LED (incl. PMU/PCA) | 130 | 31 variants have no firmware-driven LED at all |
| Notification LED | 20 | ExternalNotification default output |
| Pairing LED | 4 | All four are ELECROW ThinkNodes (M3, M4, M6) + M7 |
| Buzzer | 39 | Default ringtone/alert output |
| Ambient RGB (NeoPixel/NCP5623/LP5562/discrete) | 22 | 14 NeoPixel, 6 NCP5623, 1 LP5562, 1 discrete RGB |
| Touchscreen | 12 | |

Single-board features worth knowing about, since a regression there affects exactly one product: `LED_HEARTBEAT` and the `Battery_LED_1..4` gauge exist only on the ThinkNode M4; `LED_LORA` (RX flash) only on the ThinkNode M7; `LED_POWER_CRITICAL` only on the Seeed Mesh Tracker X1 (green heartbeat on P0.24, red critical on P0.03 — distinct pins, so the split-behavior branch in StatusLEDModule is exercised); `PIN_VIBRATION` only on the T-Deck Pro; the NeoPixel status pixels only on the T-Echo Card; the LP5562 RGBW driver only on the RAK4631 NomadStar Meteor Pro; discrete `RGBLED_*` only on the nRF52840 Dongle.

## 3. Master vs develop

Consumer-side semantics are effectively identical between branches — ButtonThread, InputBroker, StatusLEDModule, AmbientLighting, and the NodeDB defaults have only cosmetic or unrelated diffs (develop adds a portduino Linux joystick input driver and a lockdown feature that suppresses local input while the display is redacted; neither changes any variant's define semantics). All behavior drift lives in the variant files.

Five variants exist only on develop: `nrf52840/heltec_mesh_tower_v2`, `nrf54l15/nrf54l15dk` (first board of a new nRF54L15 platform — no button, no LED wired yet), `rp2040/seeed_xiao_rp2040`, `rp2350/seeed_xiao_rp2350`, and `rp2350/diy/pico2_w5500_e22`. Nothing was removed, though develop deletes `variants/nrf52840/nrf52832.ini`, an nRF52832 base build env that nothing referenced even on master — dead scaffolding.

Six shared variants changed defines within the audit scope:

| Variant | Change on develop | Behavioral effect |
|---|---|---|
| `esp32/tbeam` | `PMU_POWER_BUTTON_IS_CANCEL` added; `PMU_IRQ 35` un-commented; `VARIANT_TOUCHSCREEN 1` added | Short click of the AXP power button now turns the screen off. See finding below re: PMU_IRQ. |
| `esp32s3/tbeam-s3-core` | `PMU_POWER_BUTTON_IS_CANCEL` added | Same power-button-as-cancel behavior |
| `esp32s3/t-watch-s3` | `PMU_POWER_BUTTON_IS_CANCEL` + `PMU_IRQ 21` added | Same, and the PMU interrupt line is now wired |
| `esp32s3/t5s3_epaper` | GT911 touch poll intervals, `SCREEN_TOUCH_INT`, `TOUCH_THRESHOLD_X` 60→40, PCA9535 expander (`BOARD_PCA9535_*`) with a button mask | Substantial touch/input rework: tuned touch polling, touch deliberately excluded as a light-sleep wake source, capacitive home button gated through the I/O expander |
| `nrf52840/seeed_wio_tracker_L1` (+`_eink`) | `ADC_CTRL`/`ADC_CTRL_ENABLED HIGH` added | Battery-sense enable pin, not an LED/button change per se |

## 4. Findings

**The T-Beam PMU_IRQ reversal deserves a second look.** Master's `tbeam/variant.h` carries the comment "DO NOT ENABLE THIS because the pmuirq can cause sperious interrupts" above a commented-out `#define PMU_IRQ 35`. Develop enables exactly that define, with no comment explaining what changed to make it safe. It may well be fine now (the cancel-button feature needs the IRQ to see power-button clicks), but the old warning was neither removed with rationale nor addressed in the file — worth confirming the spurious-interrupt issue was actually fixed rather than forgotten before this ships in a release.

**Twenty-two variants define LED symbols the firmware never reads.** `LED_BLUE` (16 boards), `LED_GREEN`, `LED_RED`, and `USER_LED` are defined in many nRF52 variant files following the Adafruit core convention, but no Meshtastic application code consumes them unless they happen to alias a consumed pin (e.g., RAK4631's `LED_NOTIFICATION LED_BLUE` is live, while its `LED_GREEN` aliases the already-consumed `PIN_LED1`). The genuinely dead ones — full list in the spreadsheet's behavior column, flagged as "candidate dead defines" — include the blue LEDs on the Seeed tracker/solar boards, T1000-E, WM1110 boards, canaryone, meshtiny, TWC v4, and the green/red LEDs on the T-Echo family. Some may matter to the bootloader, but if any of those LEDs are physically populated, users get an LED that never lights; either wiring them to `LED_PAIRING`/`BLE_LED`/`LED_NOTIFICATION` or deleting the defines would make the variant files honest.

**Thirty-one variants have no firmware-driven LED and twenty-four have no button.** Some are correct by design (portduino, headless routers like the RAK3172/wio-e5 trackers, panel devices whose UI is the touchscreen — T-Deck, SenseCAP Indicator). But a few look like gaps rather than choices: `heltec_vision_master_t190`, `icarus`, `picomputer-s3`, and `nano-g2-ultra` are consumer-style devices with screens and buttons yet no status LED define (nano-g2-ultra even sets `LED_STATE_ON 0` — polarity for an LED that's never defined), and the brand-new XIAO RP2040/RP2350 variants land with neither button nor LED wired. If those boards have LEDs on the silkscreen, the defines simply haven't been added.

**LED polarity is a per-board minefield handled inconsistently.** 75 variants set `LED_STATE_ON` explicitly (a handful active-low, e.g. T-Beam's `0`), everyone else inherits active-high from `configuration.h`. The nRF52 architecture header also force-overrides polarity for two specific Arduino board defines (PCA10056, XIAO Sense). Any new active-low board that forgets the define gets an inverted heartbeat — always-on LED that blips off — which is exactly the class of bug a variant author won't catch without hardware in hand.

**The user-button event map is uniform, with two hidden exceptions.** Anyone auditing button behavior from variant files alone will miss that (a) the ThinkNode M7's screenless build carries a 15 s factory-reset hold hard-coded in `InputBroker.cpp` rather than in the variant, and (b) InkHUD builds bypass ButtonThread entirely for their UI buttons. Both are per-variant behavior living outside the variant directory — worth remembering as the audit's blind spot if the variant files are treated as the sole source.

**Sub-build conditionals hide alternate behavior.** A few variant files change LED/button defines inside `#if` blocks keyed on shield or option macros — the T-Beam + ST7796 display shield undefines `LED_POWER`/`LED_STATE_ON`/`EXT_NOTIFY_OUT` and turns the board into a touchscreen device; `tlora_v2_1_16` lets a build flag of `LED_POWER=-1` delete the LED. The spreadsheet records these with their `#if` context in the raw column but resolves the base build; treat any variant whose raw column shows conditional defines as having more than one behavioral identity.

## 5. Method and caveats

Defines were parsed from `variant.h`, any repo-local shared headers it includes (e.g. `station_common.h`), and `-D` flags in the board's `platformio.ini`, with `#if` nesting tracked and include guards excluded. First definition wins, matching the preprocessor for the base build; `#undef`-based sub-builds are recorded but not resolved (see finding above). Symbolic pin values were resolved through each variant's own `pins_arduino.h` where the repo vendors one; symbols supplied by Arduino core board packages (e.g. Heltec V3's `LED`, which the core defines as GPIO 35) remain symbolic. nRF52 port-pin arithmetic is shown evaluated — `(32 + 10) = 42` is P1.10. Behavior descriptions come from reading the consuming code on both branches (`InputBroker.cpp`, `ButtonThread.cpp`, `StatusLEDModule.cpp`, `AmbientLightingThread.h`, `ExternalNotificationModule`, `NodeDB.cpp` defaults, `nicheGraphics.h` per InkHUD variant, `WiFiAPClient.cpp`, `BluetoothStatus.h`, and the platform `architecture.h` files), not from documentation. Runtime user config (button GPIO remap, `led_heartbeat_disabled`, External Notification settings) can override any of this per device; the audit describes compile-time defaults.
