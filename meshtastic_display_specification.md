# Meshtastic Device Display Specification

**Status: Draft for discussion** · Sister document to the Meshtastic Device LED Specification · Derived from the firmware device-behavior audit (master + develop, July 2026)

This document prescribes display selection for Meshtastic devices. Like its sister, it defines user experience: the display is the single largest determinant of how a device feels to use, and this specification sets the floor for what a screen-bearing Meshtastic device should deliver. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. Device classes

Compliant devices fall into three classes, and a designer's first decision is which one they're building:

**Headless** — no display. A legitimate and well-supported class (routers, trackers, sensor nodes; roughly a fifth of the current fleet). The behavior guide's screenless button semantics and the LED specification carry the entire local user experience. A headless device is better than a device with a bad screen.

**Standard** — a monochrome OLED or e-ink display running the standard device UI (or InkHUD on e-ink). This is the fleet's center of mass: about thirty-five boards on 128×64 OLEDs and about thirty on e-ink panels, most commonly 250×122 and 200×200.

**MUI-class** — a color touchscreen device running the full Meshtastic UI. The premium experience: standalone messaging without a phone.

## 2. Monochrome OLED: the 128×64 floor

**A monochrome OLED below 128×64 is a sub-par experience and MUST NOT ship on a new design.** At 128×64 the standard UI renders a usable message view, node list, and status line; on the 128×32 strips and smaller sub-displays, the UI degrades into an ornament — the device *appears* to have a screen but the user still needs their phone for everything, which is worse than shipping headless. 128×64 (SSD1306/SH1106/SH1107 class) is both the floor and the fleet's proven standard, with roughly thirty-five boards shipping it today. A design that can only fit a smaller OLED should ship headless and lean on the LED specification instead.

## 3. E-ink displays

E-ink is its own case, and these guidelines are extracted from the thirty-plus e-ink boards currently supported rather than derived from the OLED rule.

**Panel sizes.** The fleet clusters hard around three panels: **250×122** (2.13″ — thirteen boards, the de facto standard, spanning Heltec Wireless Paper/Vision Master E213/Mesh Solar, the RAK4631 e-paper family, and Seeed's Wio Tracker L1), **200×200** (1.54″ — five boards, the compact option: T-Echo, ThinkNode M1/M5, CoreInk), and **296×128** (2.9″ — four boards, the wide option: Vision Master E290 and friends). Larger panels are proven but rarer: 240×320 (T-Deck Pro), 4.2″ 400×300, and the 5.79″ 792×272 CrowPanel. New designs SHOULD select from the three-panel cluster or go larger; the fleet's floor outliers (one board each at 192×176 and 128×80) sit below the cluster and SHOULD NOT set the pattern for new hardware — below the 200×200 / 250×122 class, e-ink runs into the same ornament problem as sub-128×64 OLEDs.

**Refresh capability is the real e-ink requirement.** The user experience of an e-ink Meshtastic device is dominated not by resolution but by refresh behavior, and the firmware treats it as a per-panel tuning problem: the panel MUST support partial/fast refresh, and the variant MUST declare tuned refresh limits for the `EInkDynamicDisplay` machinery — `EINK_LIMIT_FASTREFRESH` (consecutive fast refreshes permitted before a full refresh clears ghosting; 21 boards tune this today), the `EINK_LIMIT_RATE_RESPONSIVE_SEC` / `EINK_LIMIT_RATE_BACKGROUND_SEC` pair, and `EINK_LIMIT_GHOSTING_PX` where applicable. A panel that only supports full refresh delivers seconds-long black-flash updates on every interaction and MUST NOT ship on a new interactive design.

**UI pairing.** E-ink boards SHOULD pair with the InkHUD interface, which is built around refresh economics, and boards with a controllable backlight inherit the touch-pad backlight behavior described in the behavior guide. E-ink devices keep the full physical button semantics — the screen supplements the buttons, it does not replace them; the same holds for OLED standard-class devices.

## 4. MUI-class requirements

A device targeting MUI MUST have: **a color display**, **PSRAM**, and **a higher pixel count — at least QVGA (76,800 pixels, i.e. 320×240-class)**. It SHOULD have a **touchscreen**.

Each requirement is grounded in how MUI actually behaves. MUI is an LVGL application: its framebuffers, widget tree, and especially the offline map-tile feature are memory-hungry — the current tree literally annotates map configuration with "required for 2MB PSRAM," and no MUI board in the fleet ships without PSRAM. Color is what the UI is designed around; monochrome MUI is not a supported experience. The QVGA floor matches the fleet: every shipping MUI color device is 320×240 or better (T-Deck and Elecrow panels at 320×240, unPhone at 480×320, TLora Pager at 480×222, SenseCAP Indicator at 480×480). Touch is a SHOULD rather than a MUST because the keyboard-driven members of the class (Picomputer S3, TLora Pager) are legitimate designs — but a MUI device with neither touch nor keyboard has no way to compose a message and misses the point of the class.

E-ink is discouraged for MUI-class devices. MUI's interaction model — scrolling lists, live keyboards, animated transitions — fights e-ink refresh behavior; the one e-ink MUI device in the fleet (T-Deck Pro) is the exception that demonstrates the friction. E-ink devices should target InkHUD in the standard class, where the interface is designed for the medium.

## 5. Interaction requirements

Touchscreen devices SHOULD wake on touch (`WAKE_ON_TOUCH`), and every display device — including MUI-class — MUST retain at least one physical user button with the standard semantics from the behavior guide. The screen is never the only path to wake, shutdown, or recovery: a crashed UI, a cracked digitizer, or gloved hands must not brick the device's local controls. Display sleep is governed by the standard screen-timeout config and MUST be honored; an always-on display is a battery decision the user makes, not the designer.

## 6. Variant declaration checklist

| Hardware present | Variant MUST declare |
|---|---|
| TFT display | Panel driver + `TFT_WIDTH` / `TFT_HEIGHT` (or LGFX equivalents) |
| OLED display | Driver selection (`USE_SSD1306` / `USE_SH1106` / `USE_SH1107`) or I2C auto-detect |
| E-ink display | `USE_EINK` + `EINK_WIDTH` / `EINK_HEIGHT` + tuned `EINK_LIMIT_FASTREFRESH` / `EINK_LIMIT_RATE_*` (+ `EINK_LIMIT_GHOSTING_PX` where applicable) |
| Touchscreen | `HAS_TOUCHSCREEN`, controller config, `SCREEN_TOUCH_INT`; `WAKE_ON_TOUCH` recommended |
| MUI target | MUI build enablement + PSRAM enabled in the board build |

As with the LED specification: declare exactly what exists. A display that's populated but undeclared, or declared but absent, is a spec violation — the audit's dead-define rule applies to screens the same as LEDs.

---

*Companion documents: `meshtastic_led_specification.md` (sister prescriptive spec), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base).*
