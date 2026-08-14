# Meshtastic Device Power & Battery Specification

**Status: Draft for discussion** · Fifth prescriptive document in the device design guide series (with the LED, Display, Radio & MCU, and Button & Switch specifications) · Derived from the firmware device-behavior audit (master + develop, July 2026) and battery-ecosystem research

This document prescribes charging hardware, battery connection, and battery-state reporting for new Meshtastic devices. Power is where the fleet's user pain concentrates — wrong-polarity batteries damaging boards, misreported battery percentages, and devices that can't charge from the cable the user owns — and every rule below targets one of those failures. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. Charging connector

**New designs MUST use USB-C.** Micro-USB is over; the user's drawer, car, and travel kit are USB-C, and a device that needs the one old cable in the house is a device that arrives dead. The requirement is functional, not just mechanical: the port MUST charge from any compliant USB-C source with any compliant cable, **including C-to-C**. A USB-C port that only charges from A-to-C cables is a known, common design defect and fails this specification — test with a C-to-C cable and a modern charger before calling the port done.

**Water-resistant designs use a magnetic USB connector instead.** A sealed enclosure with a USB-C port is a contradiction — the port is the hole. Designs targeting water resistance SHOULD use a magnetic pogo-pin USB connector: the case stays sealed, there is no port cavity to flood or corrode, and the magnetic disconnect doubles as strain protection. The tether adapter that mates to it should be included with the device, since it is not a cable the user owns. (The T1000-E-class sealed trackers are the fleet's precedent for this pattern.)

## 2. Battery connector and polarity

The battery connector for removable packs SHOULD be the **JST-PH 2.0 mm**. The 1.25 mm micro connector common on older ESP32 boards (usually a Molex PicoBlade miscalled "JST") is fragile, and its pigtails are a retail-compatibility minefield; PH 2.0 is the connector the battery supply chain actually stocks.

**Polarity: this specification adopts the Adafruit convention** (what battery vendors call the "negative connection"). There is no industry standard for 2-pin JST battery polarity and the historical Meshtastic ecosystem is genuinely split — Heltec and LILYGO boards traditionally wire the opposite ("positive") convention, which is why vendors sell polarity-specific cables for them. But the *supply chain* has converged: ecosystem surveys found essentially all AliExpress-sourced 1S packs wired to the Adafruit convention, the nRF52/Adafruit-bootloader lineage (RAK WisBlock among them) already follows it, and at least one Meshtastic device maker (B&Q Consulting) migrated shipping hardware *to* it specifically for battery compatibility. The point of standardizing polarity is that the battery a user buys plugs in safely; that argument picks the convention the batteries follow. The collected design guide should carry an unambiguous polarity diagram at the connector-drawing level.

Two rules apply regardless of convention, because the installed base of both boards and batteries is mixed and mislabeled packs exist:

**Reverse-polarity protection is REQUIRED.** A reversed pack MUST NOT damage the device. This is proven practice, not gold-plating — the makers who added it ship devices that survive the wrong battery, and the ones who didn't generate the burned-board threads.

**Polarity MUST be marked** — silkscreen at the connector at minimum, on the case where the connector is user-accessible. An unmarked battery connector plus no protection is how boards die.

## 3. Safe charging

**A safe charging circuit is a MUST.** Concretely: lithium cells are charged only through a dedicated charge-management IC implementing proper constant-current/constant-voltage charging with charge termination — never through improvised circuitry, a bare diode from USB, or "the regulator handles it." The charge current MUST be appropriate for the smallest battery the device supports or ships with, not the largest the IC can do. Where the battery pack provides a thermistor, the charger SHOULD use it; where the device targets outdoor deployment, low-temperature charge cutoff is strongly recommended, because charging a frozen lithium cell is a battery-killer that solar nodes hit every winter.

Devices meant to be used while charging (which is all handhelds) SHOULD use a power-path arrangement so the system load doesn't run through the cell and confuse charge termination. Solar designs MUST use a charge controller actually designed for solar input — a bare USB charger IC fed by a panel is not a charging circuit, it's a slow fire.

Charge status MUST be communicated to the firmware and displayed per the LED Specification: PMU or fuel gauge over I2C, or the charger's STAT output on `EXT_CHRG_DETECT` (with `EXT_PWR_DETECT` for VBUS presence). No charger-driven LEDs — that rule lives in the LED spec; this section is its hardware-side mirror.

## 4. Battery state reporting

**A fuel gauge IC is HIGHLY RECOMMENDED for new designs, and the analog voltage divider is NOT RECOMMENDED.**

The divider is the incumbent — 121 of 176 variants read the battery through `BATTERY_PIN` with a per-board `ADC_MULTIPLIER` — and it is also the reason "why does my battery percentage jump around / read 100% for a day / die at 40%" is a permanent community thread. A divider gives the firmware a loaded, sagging terminal voltage and asks it to guess state-of-charge from a chemistry curve; the guess is wrong under load, wrong across cell chemistries, and wrong whenever a board ships with a miscalibrated multiplier. A fuel gauge measures charge directly and ends the guessing. The firmware's support is already frictionless: the **MAX17048** is auto-detected in the I2C scan with zero variant configuration, the **BQ27220** is supported (`HAS_BQ27220`, T-Deck Pro class), and PMU-equipped boards (AXP2101) get gauge-grade reporting from the PMU. The gauge is one small IC; it removes an entire category of user distrust.

If a design uses a divider anyway, two rules from the fleet's own better practice: the divider MUST be switchable (`ADC_CTRL`, as 33 boards already do) so the measurement network doesn't permanently drain the cell it measures — an always-connected divider is a slow self-discharge bug — and the `ADC_MULTIPLIER` MUST be calibrated against the real board, not copied from the variant the design was forked from. The audit found multiplier values that have clearly traveled between unrelated variants; every one is a board that lies about its battery.

## 5. Declaration checklist

| Hardware present | Variant MUST declare |
|---|---|
| MAX17048 fuel gauge | Nothing — auto-detected via I2C scan |
| BQ27220 fuel gauge | `HAS_BQ27220` + I2C declarations |
| PMU (AXP2101 class) | `HAS_AXP2101` (derives `HAS_PMU`) |
| Charger STAT → GPIO | `EXT_CHRG_DETECT` (+ `_MODE` / `_VALUE`); `EXT_PWR_DETECT` recommended |
| Voltage divider (discouraged) | `BATTERY_PIN` + calibrated `ADC_MULTIPLIER` + `ADC_CTRL` / `ADC_CTRL_ENABLED` |
| Magnetic / USB-C charging port | Nothing — but the §1 functional charging tests apply |

As throughout the series: declare exactly what exists, and nothing that doesn't.

---

*Companion documents: `meshtastic_led_specification.md`, `meshtastic_display_specification.md`, `meshtastic_radio_mcu_specification.md`, `meshtastic_button_specification.md` (sister prescriptive specs), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
