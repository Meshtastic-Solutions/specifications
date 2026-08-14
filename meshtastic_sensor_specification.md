# Meshtastic Device Peripheral Sensor Specification

**Status: Draft for discussion** · Sixth prescriptive document in the device design guide series (with the LED, Display, Radio & MCU, Button & Switch, and Power & Battery specifications) · Derived from the firmware device-behavior audit (master + develop, July 2026)

This document covers optional peripheral sensors on Meshtastic devices. Unlike its siblings, it is explicitly **not a limit on what may be included**: the project welcomes new data sources, the firmware's telemetry framework is built to grow, and a designer with a sensor the tree doesn't support yet should propose the driver, not drop the sensor. The rules below exist to make the sensors that *are* included work well — measure honestly, sleep properly, and integrate cleanly. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. General principles

The firmware supports roughly forty telemetry sensors today — environmental (BME280/680, SHT-series, AHT10, SCD30/4X CO₂, PMSA003I particulate, and many more), light (BH1750, VEML7700, TSL-series, LTR390 UV), power monitors (INA-series), health (MAX30102), radiation (CGRadSens), weight (NAU7802), distance (RCWL9620) — plus fourteen motion and magnetometer drivers. Nearly all are I2C parts discovered by the firmware's I2C scan: a sensor at its standard address on a scanned bus simply works, with zero variant configuration. That mechanism drives the design rules:

Prefer **I2C parts at their standard addresses on a scanned bus** — auto-detection is the integration. Address-conflicting arrangements, non-standard addresses, or sensors behind switches need variant-level declaration and are second choice. Every sensor MUST be power-manageable: a peripheral that cannot sleep, be shut down, or idle at negligible draw is a permanent tax on a battery-powered mesh node, and deep-sleep current targets are decided by the hungriest part that can't be turned off. And per the series-wide rule: declare what is soldered, nothing more — a sensor define with no sensor fails the audit's dead-define rule.

## 2. GPS

GPS is a great feature — position is half of what a mesh node is for — and it is also the most power-expensive peripheral on the board, which makes its power design the actual requirement:

**The GPS MUST have a low-power or power-off mode that retains ephemeris data.** The firmware duty-cycles GPS aggressively, and the entire benefit depends on cheap re-acquisition: a receiver that keeps its ephemeris, almanac, and RTC through sleep wakes to a hot or warm start and a fix in seconds, while a receiver that loses everything cold-starts for minutes at full acquisition power — burning more energy than the sleep saved. Concretely, the design MUST keep the module's backup domain powered (V_BACKUP pin or module standby mode) across firmware-commanded GPS sleep. The firmware already drives this on both supported patterns: a standby pin (`PIN_GPS_STANDBY`, the L76-class arrangement, 24 boards today) or software backup modes over the wire, with `PIN_GPS_EN` rail control on 59 boards; GPS.cpp explicitly manages battery-backed RAM and holds the receiver awake after a fix to finish downloading ephemeris. A design that hard-cuts all GPS power as its only "sleep" defeats that machinery and fails this rule. (The physical GPS switch from the Button & Switch Specification is the deliberate exception — assured-off is its purpose, and cold start on re-enable is the accepted price.)

**The GPS antenna MUST face skyward when the device is held or mounted naturally.** GNSS patch antennas are directional; a patch aimed at the user's palm or the ground gives up most of the sky. Determine the device's natural orientation — how it hangs on a strap, sits in a pocket lid, clips to a pack, stands on a desk — and orient the patch up in *that* attitude, not the attitude of the PCB render. Keep the patch clear of the LoRa antenna and large ground/battery masses on its sky side. For tracker-class devices whose whole job is position, this single mechanical decision does more for fix quality than any receiver upgrade.

## 3. IMU and magnetometer

Motion and heading sensing are good additions — the firmware uses them today for wake-on-motion and for compass heading in the UI, and supports a wide driver set (BMA423, BMI270, BMX160, ICM-20948, ICM-42607, LIS3DH, LSM6DS3, MPU6050, QMA6100P, STK8xxx, and BMM150 / MMC5983MA magnetometers).

Designs MAY use either a **9-axis single package** (accelerometer + gyro + magnetometer in one part, one reference frame) or a **6+3 arrangement** (a 6-axis IMU plus a separate 3-axis magnetometer). **For 6+3, the vendor MUST document the orientations**: the axis alignment of each part relative to the device body, and therefore to each other. Sensor fusion is frame math — heading computed from an accelerometer and a magnetometer that disagree about which way is X is confidently wrong, and with two separate packages nothing forces their frames to agree except the datasheet-reading the vendor did at layout time. That knowledge MUST NOT stay in the vendor's head: publish the axis conventions in the device documentation and reflect the mounting rotation in the variant so the firmware's readings are in the device frame, not the chip frame. (9-axis parts get this for free, which is a fair argument for them; 6+3 is fully acceptable *with* the documentation.)

Magnetometer placement follows the same honesty principle as §4: a magnetometer next to the speaker magnet, the vibration motor, or high-current battery traces measures the board, not the Earth. Place it away from magnetic and high-current components, and calibrate on the assembled device.

## 4. Environmental sensors

Temperature, humidity, pressure, air quality, and other environmental sensors are good — they're a large share of the telemetry catalog and a popular reason to deploy nodes.

One reminder that saves vendors from shipping a thermometer that measures itself: **a sensor sealed inside the case does not measure ambient conditions.** An enclosed volume is heated by the MCU, the radio, the display backlight, and charging; a sealed device's internal temperature reading runs several degrees above ambient and its humidity reading is the enclosure's, not the atmosphere's. If the device claims ambient measurement, the sensor needs a real path to outside air — a vented pocket isolated from board heat, a membrane port (as sealed consumer weather devices use), or an external probe. If the mechanical design can't provide that path, be honest about what's being measured: report it as device temperature, don't market it as ambient. Pressure sensing is the partial exception (cases are rarely airtight enough to matter), but temperature and humidity through a sealed wall are fiction.

Self-heating also has a duty-cycle dimension the firmware already respects: sensors read periodically, not continuously, so a well-placed sensor with a vent path reads true shortly after wake. The mechanical path is the vendor's half of the bargain.

## 5. Declaration checklist

| Sensor present | Variant declares |
|---|---|
| I2C sensor at standard address | Nothing — auto-detected by the I2C scan |
| I2C sensor at alternate address / behind a mux | Address and bus declarations for the part |
| GPS | Serial/rail pins (`GPS_TX/RX`, `PIN_GPS_EN`), plus `PIN_GPS_STANDBY` + `GPS_STANDBY_ACTIVE` where the module has a standby pin |
| IMU / magnetometer | Interrupt pin where wake-on-motion is supported; mounting orientation reflected per §3 |
| Sensor on a switched rail | The rail arrangement, so firmware sleep control works |

New sensors without existing drivers: bring the driver upstream. The telemetry framework is the extension point, and the catalog above is the precedent that it accepts new members readily.

---

*Companion documents: `meshtastic_led_specification.md`, `meshtastic_display_specification.md`, `meshtastic_radio_mcu_specification.md`, `meshtastic_button_specification.md`, `meshtastic_power_battery_specification.md` (sister prescriptive specs), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
