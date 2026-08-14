# Meshtastic device-behavior audit

Variant defines and their resolved behavior — the per-variant evidence base for the specification series. This is a markdown rendering of [`meshtastic_device_behavior_audit.xlsx`](../meshtastic_device_behavior_audit.xlsx), readable directly on GitHub; the spreadsheet remains the authoritative artifact and is the one to use for sorting and filtering.

| Sheet | Rendered as | Rows |
|---|---|---:|
| Develop | [`develop.md`](develop.md) | 176 variants |
| Master | [`master.md`](master.md) | 171 variants |
| Branch Diff | [below](#branch-diff) | 19 changes |
| Read Me | this page | — |

For the narrative analysis built on this data — findings, branch drift, and the behavioral write-up — see [`meshtastic_device_behavior_audit_report.md`](../meshtastic_device_behavior_audit_report.md).

## Source and scope

**Source of truth:** `meshtastic/firmware` source code, branches `master` and `develop` (snapshots uploaded 2026-07-13).

**Scope:** LED, button, buzzer, vibration, NeoPixel/RGB, and touch-related defines in `variants/*/*/variant.h`, locally included shared headers (e.g. `station_common.h`), and `-D` build flags in each board's `platformio.ini`.

## How “effective” values were derived

Mirrors the firmware's include chain:

1. Raw defines parsed from `variant.h` + `platformio.ini`. First definition wins; `#if` context recorded.
2. nRF52 `architecture.h` mappings applied: `PIN_LED1`→`LED_POWER` (if unset), `PIN_BUTTON1`→`BUTTON_PIN`, `PIN_BUTTON_TOUCH`→`BUTTON_PIN_TOUCH`.
3. `configuration.h` defaults applied: `PIN_BUTTON2`→`ALT_BUTTON_PIN` (unless `CANCEL_BUTTON_PIN`), `LED_STATE_ON` defaults to 1 (active-high), `BUTTON_ACTIVE_LOW`/`PULLUP` default true.
4. Symbolic pin values resolved via the variant's `pins_arduino.h` where present; nRF port pins shown as arithmetic with the evaluated GPIO number (e.g. `(32 + 10) = 42` is P1.10).

## Behavior semantics

From consuming code, identical on both branches unless noted in the report.

| Feature | Behavior |
|---|---|
| **User button** (`src/input/InputBroker.cpp` + `ButtonThread`) | With screen: click=`USER_PRESS` (cycle UI), 500 ms hold=`SELECT`, 3.9 s hold=`SHUTDOWN`. Without screen: click=`USER_PRESS`, double=`SEND_PING`, triple=`GPS_TOGGLE`, 3.9 s=`SHUTDOWN` (ThinkNode M7: +15 s=`FACTORY_RESET`). |
| **Alt/back button** | click=`ALT_PRESS`, 500 ms=`ALT_LONG`. |
| **Cancel button** | click=`CANCEL`, 4 s=`SHUTDOWN`. |
| **Touch button** | 500 ms=`BACK`; on boards with `PIN_EINK_EN` it instead momentarily lights the e-ink backlight while held. |
| **Power/status LED** (`src/modules/StatusLEDModule.cpp`) | discharging=1 ms blip per 1 s heartbeat; charging=1 s toggle; charged=solid; battery <5%=2 s burst of 250 ms blinks every 30 s. Disabled by the user config flag `led_heartbeat_disabled`. |
| **Pairing LED** | BLE unpaired=2 s blink, pairing=1 s blink, connected=solid; turns off 30 s after the last state change. |
| **Notification LED / buzzer / vibration** | Default `ExternalNotificationModule` outputs (`NodeDB.cpp` defaults: LED 1000 ms on message, buzzer PWM ringtone, vibration 500 ms with 2 s nag). |
| **LoRa RX LED** | 100 ms flash per received packet. |
| **WiFi/BLE LEDs** | On while connected. |
| **InkHUD variants** | When the InkHUD display mode is active, button handling moves to the niche `TwoButton` driver wired in the variant's `nicheGraphics.h` (see the behavior entry for each variant). |

## Caveats

- Defines inside `#if` blocks reflect the base build; shield/sub-build overrides (e.g. T-Beam + ST7796 display shield) are recorded with their condition in the raw defines but not resolved.
- Symbols provided by the Arduino core's own `pins_arduino.h` (board packages not vendored in the repo, e.g. heltec_v3's `LED`) stay symbolic.
- “Candidate dead defines” are LED symbols (`LED_BLUE`/`GREEN`/`RED`, `USER_LED`) defined by a variant but never read by Meshtastic application code on either branch (they may still matter to the bootloader or Arduino core).

## Branch diff

Every added variant and changed define between `master` and `develop`.

| Variant | Change | Define | master value | develop value |
|---|---|---|---|---|
| `nrf52840/heltec_mesh_tower_v2` | NEW VARIANT (develop only) |  |  |  |
| `nrf54l15/nrf54l15dk` | NEW VARIANT (develop only) |  |  |  |
| `rp2040/seeed_xiao_rp2040` | NEW VARIANT (develop only) |  |  |  |
| `rp2350/diy/pico2_w5500_e22` | NEW VARIANT (develop only) |  |  |  |
| `rp2350/seeed_xiao_rp2350` | NEW VARIANT (develop only) |  |  |  |
| `esp32/tbeam` | DEFINE CHANGED | `PMU_POWER_BUTTON_IS_CANCEL` | `(absent)` | `(defined, no value)` |
| `esp32/tbeam` | DEFINE CHANGED | `VARIANT_TOUCHSCREEN` | `(absent)` | `1` |
| `esp32s3/t-watch-s3` | DEFINE CHANGED | `PMU_POWER_BUTTON_IS_CANCEL` | `(absent)` | `(defined, no value)` |
| `esp32s3/t5s3_epaper` | DEFINE CHANGED | `BOARD_PCA9535_BUTTON_MASK` | `(absent)` | `0x04 = 4` |
| `esp32s3/t5s3_epaper` | DEFINE CHANGED | `SCREEN_TOUCH_INT` | `(absent)` | `GT911_PIN_INT (= 15)` |
| `esp32s3/t5s3_epaper` | DEFINE CHANGED | `TOUCH_POLL_INTERVAL_ACTIVE` | `(absent)` | `15` |
| `esp32s3/t5s3_epaper` | DEFINE CHANGED | `TOUCH_POLL_INTERVAL_ACTIVE_FAST` | `(absent)` | `8` |
| `esp32s3/t5s3_epaper` | DEFINE CHANGED | `TOUCH_POLL_INTERVAL_IDLE` | `(absent)` | `25` |
| `esp32s3/t5s3_epaper` | DEFINE CHANGED | `TOUCH_POLL_INTERVAL_RELEASE` | `(absent)` | `20` |
| `esp32s3/t5s3_epaper` | DEFINE CHANGED | `TOUCH_POLL_INTERVAL_RELEASE_FAST` | `(absent)` | `8` |
| `esp32s3/t5s3_epaper` | DEFINE CHANGED | `TOUCH_THRESHOLD_X` | `60` | `40` |
| `esp32s3/tbeam-s3-core` | DEFINE CHANGED | `PMU_POWER_BUTTON_IS_CANCEL` | `(absent)` | `(defined, no value)` |
| `nrf52840/seeed_wio_tracker_L1` | DEFINE CHANGED | `ADC_CTRL_ENABLED` | `(absent)` | `HIGH` |
| `nrf52840/seeed_wio_tracker_L1_eink` | DEFINE CHANGED | `ADC_CTRL_ENABLED` | `(absent)` | `HIGH` |

In the per-branch files these variants are flagged 🟢 (exists only on that branch) or 🟡 (defines differ between branches).
