# Meshtastic Device Revision Specification

**Status: Draft for discussion** · Tenth prescriptive document in the device design guide series (with the LED, Display, Radio & MCU, Button & Switch, Power & Battery, Peripheral Sensor, Antenna & RF, Sound & Haptics, and Recovery & Flashing specifications) · Derived from the firmware device-behavior audit (master + develop, July 2026)

Hardware gets revised. Parts go end-of-life, a layout gets an EMC fix, a cost-down lands, a connector moves three millimetres — this is normal and healthy, and nothing here discourages it. This document is about keeping a revision *cheap*: cheap for the firmware, which carries one variant per board, and cheap for the user, who has to flash the right image onto a board whose revision they usually cannot name. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. Prefer revisions the firmware cannot see

**A revision SHOULD be firmware-transparent.** The test is simple: if a stock image built for revision 1 runs correctly on revision 2, the revision is free. Everything else in this document is about the cases where that test fails.

Plenty of real change passes it. Mechanical and enclosure work, layout and EMC fixes, thermal improvements, silkscreen corrections, supplier swaps for a part with the same interface, and cost-downs that leave the programming model alone are all invisible to firmware by construction. What breaks transparency is a much shorter list, and it is worth designing around deliberately:

- **Pin assignments.** A peripheral that moves to a different GPIO is a new define and therefore a firmware change.
- **I2C addresses and bus topology.** A part that moves buses, lands behind a mux, or answers at a different address.
- **Programming interfaces.** A replacement part that is pin-compatible but register-incompatible is the expensive kind of "drop-in."
- **Polarity and active levels.** An LED or button that flips sense is a firmware change even though the schematic barely moved — see the LED Specification's inverted-heartbeat bug class.
- **Presence or absence of a peripheral.** Dropping a part is a firmware change too; the variant must stop declaring what is no longer fitted, per the audit's dead-define rule.

When a revision can be achieved without touching that list, it MUST be achieved that way. "We had to move the pin anyway" is usually a layout convenience being paid for by every user who later flashes the wrong image.

## 2. What a firmware-visible revision actually costs

The fleet is the argument. In the current tree, six product lines account for twenty variant entries — fourteen more than one-per-product:

| Product line | Variant entries in the tree |
|---|---|
| Heltec LoRa32 | `heltec_v1`, `heltec_v2`, `heltec_v2.1`, `heltec_v3`, `heltec_v4`, `heltec_v4_r8` |
| TLora 32 | `tlora_v1`, `tlora_v1_3`, `tlora_v2`, `tlora_v2_1_16`, `tlora_v2_1_18` |
| Heltec Wireless Tracker | `heltec_wireless_tracker`, `heltec_wireless_tracker_V1_0`, `heltec_wireless_tracker_v2` |
| Heltec Wireless Paper | `heltec_wireless_paper`, `heltec_wireless_paper_v1` |
| Heltec Wireless Stick Lite | `heltec_wsl_v2.1`, `heltec_wsl_v3` |
| T-Beam | `tbeam`, `tbeam_v07` |

Each extra entry is a near-duplicate variant directory to keep in sync, another build target, another row in the flasher list, and — the part that lands on users — a question they must answer before they can flash: *which revision is this?* The Wireless Tracker line illustrates the end state, with three entries and inconsistent capitalisation between them; a user looking at that list is guessing.

This is what makes revision handling a recovery concern rather than a bookkeeping one. The Recovery & Flashing Specification makes the web flasher the acceptance test: a user with a cable and the published instructions must be able to recover the device. A revision fork quietly adds a precondition to that test — the user must first correctly identify hardware that may carry no visible revision marking at all — and a wrong guess writes a mismatched image to the board.

Forking is not always wrong. A change of MCU, radio, or flash class is a *new board* and MUST get its own variant; the Heltec line crossing from ESP32 to ESP32-S3 between v2.1 and v3 is exactly that, and no amount of runtime detection should paper over it. The rule in this document applies to revisions of the same product, where the user reasonably expects one firmware.

## 3. Keep the delta minimal

Where firmware must change, **the change SHOULD be confined to the variant's defines**, not spread into driver selection, build flags, partition layout, or a new platform target. A revision whose entire footprint is a handful of `#define` values is one a single image can absorb at runtime (§4); a revision that changes which drivers compile is one that forces a second build.

Changes that ripple deserve particular scrutiny at design review, because their cost is not visible on the schematic: moving a peripheral to a bus that requires a different driver, adding a part that changes the build's memory footprint, or altering the flash map. If a proposed revision does any of these, the right question is whether the benefit survives being weighed against a permanent second build target.

## 4. The revision MUST be detectable at boot

**Where a revision is firmware-visible, the board MUST provide a way for firmware to determine, at boot, which revision it is running on.** This is what lets one image serve every revision of a product, which is the outcome §2 is protecting. A firmware-visible revision with no detection mechanism is the case this specification exists to prevent: it forces the fork, and it hands the user the identification problem.

In order of preference:

**Implicit detection — the change is its own marker.** The best mechanism costs nothing, because the thing that changed announces itself. The worked example is a keyboard attached over I2C: in revision 2 the keyboard answers at a different I2C address, so the same bus probe that finds the keyboard also establishes which revision the board is. No extra part, no extra pin, no extra BOM line — the detection is a side effect of the change. Meshtastic hardware is unusually well suited to this, because the firmware already probes I2C at startup for some forty telemetry parts and auto-detects them at their standard addresses; a revision that swaps or re-addresses an auto-detected peripheral gets its marker for free. Where a revision touches an I2C part at all, arranging for the address to differ is the cheapest detection available and SHOULD be the first option considered.

**An explicit revision strap.** Where the change has no natural signature, dedicate one or more GPIOs strapped high or low by resistors, or a resistor divider into an ADC channel, encoding a revision number. This costs a pin and two passives and is unambiguous, extensible to later revisions, and readable before anything it gates. Designs that anticipate a long product life SHOULD fit a strap on revision 1 — retrofitting one is itself a firmware-visible revision, which is the problem eating itself.

**Dedicated identification hardware** (an I2C EEPROM, a 1-wire ID part) is available but rarely justified at this scale; prefer it only where a board already carries such a part for another reason.

Three requirements apply to whichever mechanism is chosen:

- Detection MUST be non-destructive and MUST NOT hang when the expected hardware is absent. A probe that blocks forever on a missing part converts a revision mismatch into a dead device.
- Detection MUST be performed before anything it gates, and MUST degrade safely on an unrecognised result. Firmware meeting a revision newer than itself SHOULD fall back to the most conservative supported behavior and come up degraded — never dark. This is the Recovery & Flashing Specification's prime directive applied to revision handling: an older image on newer hardware is a normal event, not a brick.
- The detected revision SHOULD be surfaced — logged at boot and exposed in device metadata — so that a support conversation can establish the revision without asking the user to open the case.

## 5. Mark it on the board, and never ship a silent revision

**The revision MUST be marked on the board in a place a user can read without disassembly**, and stated in the product documentation. This is the same discipline the series applies to the antenna band label and the `RST` legend: the information a user needs in order to act correctly must be visible at the moment they act.

The failure to avoid above all others is the **silent revision** — a board whose hardware changed behind identical markings and identical firmware-visible behavior. It defeats everything above at once: firmware cannot detect it, the label cannot disambiguate it, and support cannot establish which one a user is holding, so every report from the field becomes ambiguous. If a change alters behavior in any way a user or the firmware could observe, the revision marker MUST be bumped and §4's detection MUST be provided. A revision that is genuinely invisible in behavior needs neither — that is the §1 ideal, and it is the only case where shipping without a detectable marker is acceptable.

## 6. Declaration checklist

| Situation | Variant declares |
|---|---|
| Firmware-transparent revision | Nothing — this is the goal |
| Revision detected via an existing probe (e.g. a distinct I2C address) | Nothing beyond the peripheral's own declarations — the probe is the detection |
| Revision strap pin(s) | The strap GPIO(s) and the revision encoding they resolve to |
| Revision-conditional pin or peripheral | A runtime branch on the detected revision, inside one variant — not a forked variant |
| New MCU, radio, or flash class | A new variant — this is a new board, not a revision |
| Board marking and documentation | Nothing in the variant — silkscreen and documentation duties |

As throughout the series: declare exactly what exists. On a board with more than one revision in the field, "what exists" is a function of the detected revision, and the variant must not declare hardware that a given revision does not have.

---

*Companion documents: `meshtastic_led_specification.md`, `meshtastic_display_specification.md`, `meshtastic_radio_mcu_specification.md`, `meshtastic_button_specification.md`, `meshtastic_power_battery_specification.md`, `meshtastic_sensor_specification.md`, `meshtastic_antenna_rf_specification.md`, `meshtastic_sound_haptics_specification.md`, `meshtastic_recovery_flashing_specification.md` (sister prescriptive specs), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
