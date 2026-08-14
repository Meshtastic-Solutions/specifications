# Meshtastic Device LED Specification

**Status: Draft for discussion** · Derived from the firmware device-behavior audit (master + develop, July 2026)

This document prescribes how status LEDs on Meshtastic devices should be colored, wired, and declared in firmware. Where the current behavior guide describes what existing boards *do*, this specification says what new boards *should do*. The goal is that a person can pick up any compliant Meshtastic device and read its LEDs without consulting a manual, and that hardware designers can wire LEDs without guessing which firmware define makes them work.

The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. Color assignments

| Color | Function | Firmware define | Behavior |
|---|---|---|---|
| **Red** | Device status (heartbeat, charge, critical) | `LED_POWER` | ~1 ms blip once per second on battery; 1 s blink charging; solid when charged; fast flicker below 5% battery |
| **Green** | LoRa activity | `LED_LORA` | 100 ms flash per received packet (TX flash: see §8) |
| **Blue** | Bluetooth status | `LED_PAIRING` | 2 s blink unpaired, 1 s blink pairing, solid connected, auto-off 30 s after last state change |
| **Amber** | New message / notification | `LED_NOTIFICATION` | 1 s on message arrival by default; user-configurable via the External Notification module |

A *heartbeat-only* red LED (`LED_HEARTBEAT`, which blips on battery and shows nothing else) is discouraged for new designs: it silently drops charge and critical-battery indication, and the audit shows those states are the ones users actually ask about. Boards SHOULD wire red as the full status LED (`LED_POWER`); users who prefer the minimal heartbeat presentation should get it as a runtime device setting rather than a hardware decision (see §8). `LED_HEARTBEAT` remains supported for existing boards.

The assignments follow convention. Red is the universal "device alive" color and, per §3, is reserved exclusively for the GPIO status LED. Green as activity and blue as Bluetooth match user expectations from decades of consumer hardware. Amber is the conventional attention color and is visually distinct from all three primaries.

Color is never the sole carrier of meaning: each function also has a distinct temporal signature (blip vs. flash vs. cadenced blink vs. sustained pulse) and a fixed placement order (§6), so the scheme remains readable under color-vision deficiency — red/amber/green discrimination in particular cannot be assumed.

## 2. The red rule

**Red LEDs are GPIO-controlled status LEDs, and nothing else.** A red LED on a compliant device MUST be driven directly from a GPIO through a series resistor, with no transistor, driver IC, charge-pump, or hardware auto-blink circuit between the pin and the LED. Conversely, the status/heartbeat function MUST NOT be assigned to any other color.

The reasoning is diagnostic: red blipping once per second must mean, unambiguously, "the firmware main loop is alive and this exact GPIO is toggling." Any intermediate circuit breaks that guarantee — the audit found boards where charger-IC hardware blinks the status LED on its own (`POWER_LED_HARDWARE_BLINKS_WHILE_CHARGING`), making it impossible to tell firmware state from charger state. That class of ambiguity is what this rule eliminates. The heartbeat's ~0.1% duty cycle also makes it the one indication with effectively zero battery cost, which is why red is the LED every battery-powered board should have even when all others are omitted.

## 3. Drive and polarity (all status LEDs)

All four status LEDs MUST be driven directly by a GPIO. Active-high wiring (GPIO sources current; `LED_STATE_ON` = 1) is preferred: it is how roughly four out of five existing boards are wired (103 of the 129 boards with discrete firmware LEDs on develop — 46 explicitly, 57 via the firmware default) and it matches that default, so a variant file that says nothing gets correct behavior. Active-low (GPIO sinks; the remaining 26 boards, mostly the Heltec T114/Mesh Node and T-Echo families) is permitted, but then `#define LED_STATE_ON 0` in the variant is mandatory — the audit's inverted-heartbeat bug class (an always-on LED that blips off) is exactly what happens when an active-low board omits it.

Brightness is a user-experience choice, not a component choice: status LEDs should be clearly visible indoors and not distracting in a dark room. The heartbeat blip in particular should read as a glance-confirmation, not a strobe. Component selection and current limiting are left to the designer.

No status LED may be powered from a switched rail that the firmware turns off in low-power states, or the heartbeat dies with the peripheral rail and falsely signals a crashed device.

## 4. Charge status flows through the firmware

There is no standalone charger LED. The charger's status MUST be communicated to the firmware, and the firmware indicates charge state on the red power LED per §1 — one LED tells the entire power story, with one consistent set of patterns across every compliant device.

Concretely: the charger IC's status output MUST NOT drive an LED directly. On boards with a PMU or fuel gauge, charge state arrives over I2C and no extra wiring is needed. On boards with a simple charger IC, its STAT output MUST be routed to a GPIO and declared in the variant as `EXT_CHRG_DETECT` (with `EXT_CHRG_DETECT_MODE`/`EXT_CHRG_DETECT_VALUE` matching the wiring); routing VBUS presence to a second GPIO as `EXT_PWR_DETECT` is RECOMMENDED so the firmware can distinguish "on USB, done charging" from "on battery." All of these are existing, consumed firmware mechanisms — seven boards in the current fleet already wire `EXT_CHRG_DETECT`.

This rule is what makes the red LED's charging and charged patterns trustworthy, and it retires `POWER_LED_HARDWARE_BLINKS_WHILE_CHARGING` entirely: no new design may let charger hardware blink an LED on its own.

## 5. Reduced and extended configurations

Not every board has room for four LEDs. Omit in reverse priority order:

Four LEDs: red, amber, green, blue — the full specification. Three: drop blue (Bluetooth state is visible in the client app, which a BLE user by definition has open). Two: drop green (mesh activity is diagnostic, not operational). One: **red**, wired as the full status LED (`LED_POWER`) — a single-LED board keeps heartbeat, charge, and critical indication, because "is it alive and does it have power" are the questions no app can answer for a device that won't connect.

Going the other direction, boards MAY add a **four-LED battery gauge** (`Battery_LED_1` through `Battery_LED_4`; precedent: ThinkNode M4). The firmware behavior is already defined: the gauge appears for 10 seconds after any button press and whenever the power LED is lit for charge indication (pulsing in step with the charging blink, steady alongside a fully-charged indication), then goes dark — so it costs nothing at idle. Segment 1 is a floor that always lights while the gauge is shown; segments 2–4 light at 25%, 50%, and 75% charge, making the display read one-to-four at a glance. Gauge rules: all four segments MUST be the same color, because a gauge reads by count, not color; that color MUST NOT be red (red is reserved for the firmware status authority of §2) and SHOULD NOT duplicate a status color sitting adjacent to the status group — white is a good choice. The gauge is placed as its own linear group, oriented so segment 1 is nearest the user's natural "empty" end (bottom or left), separate from the status row, and follows the same drive rules as §3.

## 6. Placement and labeling

Where multiple status LEDs are present they MUST be grouped, ordered **red, amber, green, blue** (descending wavelength — trivially memorable), and individually silkscreened: `PWR`, `MSG`, `LoRa`, `BT`. The fixed order is the second channel that makes the scheme colorblind-safe.

## 7. NeoPixel alternative

Space-constrained boards MAY implement this specification on addressable RGB pixels instead of discrete LEDs, following the existing `NEOPIXEL_STATUS_*` mechanism (precedent: T-Echo Card), with pixel colors matching §1 exactly. Note the trade-off honestly: a NeoPixel draws standby current even when dark and requires its rail powered to show a heartbeat, so discrete LEDs remain preferred wherever they fit. Ambient/decorative RGB lighting (the AmbientLighting module) is orthogonal to this spec and MUST NOT be conflated with status indication.

## 8. Required firmware work

Three gaps in the current firmware must close before this spec is fully realizable, and one is a selection decision:

**Heartbeat-only as a user setting.** This spec discourages heartbeat-only *hardware* but the minimal presentation should remain available to users who want it. The device config already has `led_heartbeat_disabled` (suppress the heartbeat, keep charge indication); the complement doesn't exist — a setting that suppresses charge/charged indication and leaves only the heartbeat blip on `LED_POWER`. Adding it makes the presentation a runtime choice instead of a wiring decision.

**Critical-battery indication on existing heartbeat-only boards.** Today the <5% fast-flicker is routed through the charge-LED state (`LED_POWER`, PMU, or PCA paths). A board that defines only `LED_HEARTBEAT` (e.g. the ThinkNode M4) gets no visible critical indication — during flicker windows the heartbeat is merely suppressed, so a dying battery looks like a 2-second heartbeat pause every 30 s. StatusLEDModule needs a small patch: when no charge-LED output exists, route the critical flicker to `LED_HEARTBEAT`. Lower priority now that new designs are steered to `LED_POWER`, but it fixes shipped hardware.

**LoRa TX flash.** `LED_LORA` currently observes receive only. "LoRa activity" should flash on transmit as well; the module already shares the radio thread, so this is an added observer, not a redesign.

**`LED_PAIRING`, not `BLE_LED`, is the blue LED.** The tree contains two Bluetooth indicators. `BLE_LED` is solid-on while connected — a permanently lit blue LED is both a battery drain and a nighttime nuisance. `LED_PAIRING` has the correct semantics (state-distinguishing cadences plus 30-second auto-off) and is what this spec prescribes; `BLE_LED` should be considered deprecated for new designs.

## 9. Variant file checklist

A compliant variant declares exactly the LEDs that exist, and nothing else:

| Hardware present | variant.h MUST contain |
|---|---|
| Red status LED | `#define LED_POWER <gpio>` (`LED_HEARTBEAT` only for heartbeat-only designs, discouraged) |
| Amber message LED | `#define LED_NOTIFICATION <gpio>` |
| Green LoRa LED | `#define LED_LORA <gpio>` |
| Blue Bluetooth LED | `#define LED_PAIRING <gpio>` |
| Charger STAT → GPIO (no PMU/fuel gauge) | `#define EXT_CHRG_DETECT <gpio>` (+ `_MODE`/`_VALUE` per wiring); `EXT_PWR_DETECT` recommended |
| Battery gauge (optional, §5) | `#define Battery_LED_1 <gpio>` … `Battery_LED_4 <gpio>` |
| Any active-low LED | `#define LED_STATE_ON 0` |

Two audit-motivated rules. First, **no dead defines**: the audit found 22 boards declaring LED symbols (`LED_BLUE`, `LED_GREEN`, `LED_RED`, `USER_LED`) that no firmware code reads — populated LEDs that never light. Every silkscreened status LED must map to a consumed define, and no define may be declared for an LED that doesn't exist. Second, **no orphan hardware**: an LED on the board with no define is a spec violation equal to a define with no LED. The audit spreadsheet's per-variant behavior column is the reference for which defines are actually consumed.

---

*Companion documents: `meshtastic_standard_behavior_guide.md` (descriptive — what current devices do) and `meshtastic_device_behavior_audit.xlsx` / audit report (per-variant evidence base).*
