# Meshtastic Device Antenna & RF Specification

**Status: Draft for discussion** · Seventh prescriptive document in the device design guide series (with the LED, Display, Radio & MCU, Button & Switch, Power & Battery, and Peripheral Sensor specifications) · Derived from the firmware device-behavior audit (master + develop, July 2026) and RF-ecosystem research

This document prescribes antenna connectors, labeling, and RF integration for new Meshtastic devices. The antenna path is the least visible part of a device and the most consequential: it decides link budget for the node *and* — because every node relays — for the mesh around it. It is also the top generator of avoidable user pain: wrong connector, wrong band, damaged port, deaf stock antenna. Every rule below targets one of those. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. Connector: standard SMA, and not RP-SMA

**Devices with an external antenna MUST use standard SMA — device-side female (center socket), antenna-side male (center pin).** This is the established LoRa and Meshtastic convention; RP-SMA is the WiFi-heritage connector (created for old FCC antenna-swap compliance) that keeps leaking into the ecosystem and generating the perpetual "my antenna doesn't fit / screws on but has no signal" support thread. The two are visually identical, mechanically threadable in some mismatches, and electrically incompatible — an RP-SMA-to-SMA mismatch can leave a node transmitting into a connector with no center contact, which is a §2 event. A mixed fleet forces users into adapters, and every adapter is insertion loss plus mechanical leverage on the port. New designs end the mixing: standard SMA, no exceptions for styling or parts-bin convenience, and the connector type stated explicitly in the product documentation because retail mislabeling is rampant.

**U.FL/IPEX is internal-only.** These connectors are rated for on the order of tens of mating cycles and tear off boards under cable strain; they are board-to-bulkhead pigtail connectors, not user-facing ports. A U.FL joint inside the enclosure MUST be mechanically secured (retention clip, adhesive, or routing that prevents the cable from loading the connector), and the user-facing end of such a pigtail is the SMA bulkhead above. Sealed, non-serviceable devices with fully internal antennas (the T1000-E-class trackers) are exempt from the connector rules entirely — there is no user-facing RF connector to standardize.

**The SMA port MUST be enclosure-mounted or enclosure-supported.** Antennas are levers: a half-wave whip on a board-soldered edge connector applies its bending moment to solder joints, which is the RF version of the Button Specification's detaching-switch failure. Mount the connector to the case (bulkhead) or brace a board-mounted connector with the enclosure so antenna torque loads the housing, never the pads.

## 2. Never transmit into nothing

Users unbox devices and power them on before attaching the antenna — reliably, repeatedly, regardless of what the quick-start says. Transmitting into an open connector reflects the full transmit power back into the final stage, and on PA-equipped designs that is a hardware-killing event; the transceivers in use have no VSWR sensing, so the firmware cannot detect or prevent it.

Because the failure can't be caught in software, the mitigation is layered and mostly the vendor's: a **prominent warning at the connector** ("attach antenna before power-on") on the device or an unmissable unboxing insert; standard-power designs SHOULD be built to survive brief antenna-less operation (SX126x-class modules with sane matching generally do — verify it, don't assume it); and PA-class designs, where survival can't be assured, MUST make the warning impossible to miss and SHOULD ship with the antenna pre-attached or packaged so that attaching it is physically the first step.

## 3. Band identification

**The operating band MUST be labeled on the device at the antenna connector, and on the included antenna itself.** "Bought the 868 device in a 915 country" and "grabbed the 433 antenna for the 915 node" are perennial support threads, and both are label problems: band is invisible from the outside of otherwise-identical hardware. Concretely: the frequency band (915/868/433/2.4 GHz, or the supported range for multi-band LR11x0 designs) appears on the case near the connector and on the antenna body or a permanent antenna tag — so a mismatch is visible at the moment of assembly, not after a week of terrible range. Product listings MUST state the band in the title, not the fine print. (Regulatory region is the firmware's job at setup; the label's job is making the *hardware's* band knowable without a spectrum analyzer.)

## 4. The stock antenna is part of the product

The antenna in the box defines the device's out-of-box experience and its contribution to every mesh it joins — a deaf node isn't just a bad product experience for its owner, it's a degraded relay for everyone around it. The included antenna MUST actually be tuned for the device's band (the notoriously untuned generic whip is a real and common shipping failure), and the vendor SHOULD publish measured antenna performance for the assembled device rather than the antenna datasheet alone.

**Internal and PCB-trace antennas are acceptable — and for some devices, right.** Form factor and weather-proofing are legitimate reasons to go internal: no whip to snag on a pack strap, no port to seal against rain, no connector to torque (the sealed-tracker class exists because of exactly this). The condition attached is that they be **well tested**, because an embedded antenna's performance is set by everything around it — the board stackup, the ground plane it's cut into, the battery beside it, the enclosure over it, and the hand holding it — none of which appears in the antenna's datasheet. "Well tested" means validated on assembled, production-representative units, not an evaluation board: the antenna MUST be tested in the device's *natural hold* — the recurring theme of this series — since the user's hand is part of the antenna's environment. An internal antenna placed where the natural grip covers it loses its performance to the palm; place it where the grip isn't (top edge, away from the battery mass and ground planes), and validate range with the device held, worn, and pocketed the way users actually carry it, not on a foam test stand. An internal antenna usually gives up something against a tuned external whip; that's an acceptable trade when it's a *measured* trade — publish honest range expectations, and let the sealed form factor be the selling point rather than an unstated performance cut.

## 5. Placement and coexistence

Three antennas commonly share these devices — LoRa, GNSS, and BLE/WiFi — and they are placed as a system: the GNSS patch faces sky in the natural hold with the LoRa antenna kept off its sky side (per the Sensor Specification), the module's BLE/WiFi chip antenna keep-out zones are honored (a keep-out with a battery in it is a disconnected antenna with extra steps), and the LoRa antenna is positioned so its near field isn't dominated by the battery, the display, or the hand. For desktop/base designs, assume the user will wall-mount or shelf-mount in whatever orientation the case invites — if performance depends on orientation, make the case invite the right one.

## 6. Frequency reference: TCXO required

**New designs MUST use a TCXO as the radio's frequency reference, not a simple crystal oscillator.** This is a Meshtastic-specific requirement with a Meshtastic-specific reason: this protocol sends *long* packets. Where LoRaWAN moves small payloads under tight dwell-time discipline, Meshtastic's default presets run high spreading factors with payloads up to a full 255-byte frame — seconds of continuous airtime per packet. LoRa demodulation tolerates only a limited frequency error relative to the signal bandwidth, and the killer isn't static offset (which the receiver acquires at the preamble) — it's *drift during the packet*: the transmit burst heats the board, a bare crystal walks with that temperature ramp, and a frequency that was fine at the preamble has slid by the final symbols. The longer the packet, the more thermal ramp it contains, which is exactly why a reference that's adequate for LoRaWAN's short bursts corrupts Meshtastic's long ones — showing up in the field as nodes that work on the bench, then lose long messages outdoors in winter sun or summer heat, at exactly the slow presets where each retry is most expensive.

The fleet has already voted: 136 of 176 variants declare a TCXO voltage. A new design using a bare crystal is choosing the failure mode the ecosystem has moved past, to save fractions of a dollar on the most important component in the RF chain. Radio modules with an integrated TCXO satisfy this requirement — most current SX1262 modules ship one — and the variant MUST declare the TCXO arrangement correctly (`*_TCXO_VOLTAGE`, or the explicit no-TCXO/XTAL declaration on legacy hardware), because a wrong TCXO declaration produces the same off-frequency behavior as a bad reference.

## 7. High-power (PA/FEM) designs

Amplified designs are supported — the tree carries FEM-equipped boards and a dedicated `LoRaFEMInterface` — and they take on extra duties in exchange for the wattage:

**The gain MUST be declared to the firmware.** The firmware's transmit-power setting is a user-facing and regulatory-facing number; an undeclared external PA makes every value a lie and can push a legal configuration over regional limits. The declaration mechanisms exist and MUST be used: `TX_GAIN_LORA` for FEM gain accounting in `RadioInterface`, and the `SX126X_MAX_POWER` / `SX128X_MAX_POWER` caps (15 and 6 boards respectively today) so the firmware cannot command more drive than the design tolerates. Defaults as shipped MUST be within the limits of the region the device is sold into. PA-class devices also inherit the §2 hardened-warning duty and add a thermal one: sustained transmit at full power (a busy router node) must not cook the device — validate the thermal path at router duty cycles, not messaging duty cycles.

## 8. Declaration checklist

| Hardware present | Variant MUST declare |
|---|---|
| LoRa transceiver | Radio selection + `*_MAX_POWER` cap where the design's limit is below the chip's |
| TCXO (required, §6) | `*_TCXO_VOLTAGE` declaration matching the module (136 boards today) |
| LR11x0 / switched-path radio | Correct `rfswitch.h` table (25 boards today) |
| External PA / FEM | `TX_GAIN_LORA` gain accounting + FEM control pins |
| Antenna connector / band | Nothing in the variant — §1 and §3 are case, label, and documentation duties |

As throughout the series: the RF plumbing declarations are consumed code paths, and the audit's dead-define rule applies — an `rfswitch.h` copied from the wrong board is a node that transmits into the wrong path.

---

*Companion documents: `meshtastic_led_specification.md`, `meshtastic_display_specification.md`, `meshtastic_radio_mcu_specification.md`, `meshtastic_button_specification.md`, `meshtastic_power_battery_specification.md`, `meshtastic_sensor_specification.md` (sister prescriptive specs), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
