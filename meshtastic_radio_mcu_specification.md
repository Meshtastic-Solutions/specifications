# Meshtastic Device Radio & MCU Specification

**Status: Draft for discussion** · Third prescriptive document in the device design guide series (with the LED and Display specifications) · Derived from the firmware device-behavior audit (master + develop, July 2026)

This document prescribes LoRa transceiver and microcontroller selection for new Meshtastic devices. The supported-hardware lists below are pulled from the firmware source — the radio interface implementations in `src/mesh/` and the platform trees under `variants/` — and the minimum-resource rules are derived from what the current fleet actually ships and what the firmware demonstrably requires. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. LoRa transceiver

### Supported by the firmware today

The firmware carries radio interfaces for: the **SX126x family** (SX1262, SX1268, LLCC68), the **SX127x family** (SX1276/RFM95-class, via the RF95 interface), the **SX128x family** (SX1280, 2.4 GHz), the **LR11x0 family** (LR1110, LR1120, LR1121), the **LR20x0 family** (LR2021 — newly added), and the **STM32WLE5** integrated radio core (SX126x-class). Fleet usage on develop: SX1262 appears in 129 variants and is the overwhelming standard; SX1268 in 26 (the 433/490 MHz variant of the same family); LLCC68 in 9; SX1280 in 15; the LR11x0 parts in 18 combined; and the SX1276/RF95 class in 50 — but 24 of those are RF95-*only* boards, two-thirds of them on the original ESP32, i.e. the legacy cohort.

### Rules for new designs

**SX1276 (RFM95 and equivalents) is discouraged for new designs, and anything older MUST NOT be used.** The SX1276 is a 2015-era part: poorer sensitivity and much higher receive current than the SX126x generation, lower maximum output power, and it anchors exactly the legacy board cohort this guide series is trying to move past. Its predecessors and siblings (SX1272/SX1273, older RFM modules, and any FSK-only part) are prohibited outright. Existing SX1276 boards remain supported by the firmware; this rule is about what gets designed next.

New designs SHOULD use the **SX1262** (or SX1268 for 433/490 MHz targets) as the default choice — it is what 129 of 176 current variants ship and where firmware attention concentrates. The **LR11x0 family** is a fully supported premium alternative (multi-band capability, GNSS/WiFi scanning), and the **SX1280** is the correct part for 2.4 GHz designs. The **LLCC68** MAY be used in cost-driven designs with one explicit caution: it supports a reduced spreading-factor range compared to the SX1262, so the design MUST be verified against the modem presets and regional settings it will actually be sold into before committing. The **STM32WLE5's** integrated core is radio-compliant (it is SX126x-class silicon) — the constraints on that chip are MCU-side, covered below.

Whatever the transceiver, the RF switch configuration (`rfswitch.h` on LR11x0/newer designs) and TCXO arrangements are part of the variant and MUST be declared correctly — the audit's dead-define rule applies to radio plumbing as much as to LEDs.

## 2. Microcontroller

### Supported by the firmware today

Platform trees on develop, with variant counts: **ESP32-S3** (59 — the ESP32-side standard), **original ESP32** (32 — the legacy cohort), **nRF52840** (55 — the low-power standard), **RP2040** (12) and **RP2350** (5), **ESP32-C3** (7), **ESP32-C6** (3), **ESP32-S2** (2), **STM32WL** (7), **nRF54L15** (2 — new platform, first DK board landed on develop), **ESP32-P4** (build scaffolding only, no boards yet), and **native/portduino** (Linux, 3 targets).

### Rules for new designs

**The original ESP32 MUST NOT be used in new designs.** The ESP32-S3 and similar current-generation chips are the acceptable ESP32-side choices. The S3 is the default: native USB, current silicon, PSRAM support, and the largest active variant population. The **C3 and C6** MAY be used for small, cost-driven designs. The **S2** SHOULD NOT be used: it has no Bluetooth, and a Meshtastic node that can't pair with a phone is crippled for the primary use case — the two S2 boards in the tree are novelty hardware, not a pattern. The **P4** is not yet a target (scaffolding only, and it needs a companion chip for wireless).

On the low-power side, the **nRF52840** is the standard and the only supported nRF52 — the nRF52832 (512 KB flash) is not viable, and develop has already deleted its last build scaffolding. The **nRF54L15** is an emerging platform; treat it as experimental until real boards ship. **RP2040 and RP2350** are supported for headless and utility designs. The **STM32WL** is a special case: it runs a heavily reduced build with Bluetooth, screen support, GPS, MQTT, and most modules excluded — it is permitted *only* for bare mesh-node/repeater designs where that feature set is the point, and MUST NOT be chosen for anything user-facing.

## 3. Minimum flash and memory

Derived from the fleet and the firmware's own partition and exclusion arrangements:

| Platform | Minimum | Recommended | Basis |
|---|---|---|---|
| ESP32-S3 / C3 / C6 | 4 MB flash (hard floor) | **8 MB**; 16 MB for MUI-class | The 4 MB partition layout is the tree's own marked-legacy squeeze: asymmetric OTA slots and minimal filesystem. 8 MB is the fleet's modal size (25 boards on the stock 8 MB table) and gives clean dual-OTA plus filesystem; 16 MB (19 boards) is where MUI devices with offline maps live. MUI-class additionally requires PSRAM per the Display Specification. |
| nRF52840 | 1 MB flash / 256 KB RAM (silicon-fixed) | — | This *is* the floor: the fleet's nRF52 boards all run in ~800 KB of application flash after SoftDevice and bootloader. Nothing smaller is supported — the 512 KB nRF52832's removal from the tree is the precedent. |
| RP2040 / RP2350 | 2 MB QSPI flash | 4 MB+ | 2 MB is the Pico-class baseline the current boards ship; more costs little and buys OTA/filesystem headroom. |
| STM32WL | 256 KB / 64 KB (silicon-fixed) | — | Viable only for the stripped repeater build described above; the exclusion list is the specification. |

The general principle behind the table: a new design MUST have room for over-the-air update (two application slots) plus a persistent filesystem for settings and the node database, and SHOULD have headroom for the firmware's steady growth — the 4 MB ESP32 layout exists because boards shipped without that headroom, and its strained partition map is the cautionary tale. When in doubt, the next flash size up is the cheapest insurance in the entire bill of materials.

## 4. Selection quick reference

| Building… | MCU | Radio |
|---|---|---|
| Standard handheld / node (screen, BLE) | ESP32-S3 or nRF52840 | SX1262 |
| Lowest-power tracker / sensor | nRF52840 | SX1262 (LR11x0 if GNSS-scan wanted) |
| MUI-class device | ESP32-S3, 16 MB + PSRAM | SX1262 / LR11x0 |
| 433/490 MHz regional design | per above | SX1268 |
| 2.4 GHz design | per above | SX1280 |
| Bare repeater / infrastructure node | STM32WL, RP2040/RP2350, or nRF52840 | SX1262-class |

---

*Companion documents: `meshtastic_led_specification.md`, `meshtastic_display_specification.md` (sister prescriptive specs), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
