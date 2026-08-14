# Meshtastic Device Recovery & Flashing Specification

**Status: Draft for discussion** · Ninth prescriptive document in the device design guide series · Derived from the firmware device-behavior audit (master + develop, July 2026)

This document prescribes firmware recovery and flashing for new Meshtastic devices, and it has one prime directive from which everything else follows:

**The device must be essentially unbrickable.**

Meshtastic users flash firmware — alphas, betas, forks, their own builds — as a normal part of owning the device; the project's culture assumes it and the web flasher exists to invite it. A device that can be killed by a bad flash punishes exactly the participation the project wants. So the standard is: **no firmware image delivered through any normal flashing channel — web flasher, serial, UF2 drag-and-drop, or OTA — may leave the device in a state the user cannot recover with a cable and a documented gesture.** No disassembly, no soldering, no JTAG probe, no depot return. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. What "essentially unbrickable" means in practice

The property is architectural, not aspirational, and it rests on one rule: **the recovery path MUST NOT be erasable by the payload it recovers from.** Recovery lives in mask ROM or in a write-protected bootloader region that application firmware cannot touch; entry into recovery is a physical gesture (button held at reset, double-tap, boot-select) that works regardless of what the application flash contains — including garbage, including nothing. If a design's recovery depends on the application behaving, the design is brickable by definition.

The supported MCU platforms all provide this property natively, which is part of why the Radio & MCU Specification blesses them — the vendor's job is to not defeat it:

**ESP32-family**: the ROM serial bootloader is un-erasable by construction; holding BOOT through reset always yields download mode. The vendor's obligations are an accessible BOOT button and a working USB-serial path. **nRF52840**: the UF2 bootloader (double-tap reset → device appears as a USB drive, drag the firmware on) is the friendliest recovery in the fleet; it MUST live in the protected region, and vendors SHOULD ship a current bootloader version — several shipped devices carry ancient bootloaders with known flashing bugs, which is a self-inflicted wound on the recovery path. **RP2040/RP2350**: BOOTSEL mass-storage recovery is in mask ROM; expose the BOOTSEL button per §3. **STM32WL** has no USB and is the platform's weak spot: a design MUST provide a documented recovery path (system-bootloader UART pins or SWD pads, §5) and accept that this is one more reason the platform is fenced to infrastructure devices.

## 2. USB must be a data port, and the web flasher is the acceptance test

Recovery arrives over USB, so **the USB connector MUST have working data lines** — a charge-only USB port turns every soft failure into a hard one. This sounds too obvious to state; shipped devices prove otherwise. On MCUs with native USB (ESP32-S3, nRF52840, RP2xxx) use it; where a serial bridge is used instead, it MUST support the standard flashing tools including their reset/boot auto-sequencing.

The concrete acceptance test: **the device MUST be flashable and recoverable via the official Meshtastic web flasher** (or a documented equivalent for platforms the flasher doesn't cover), by a user following the device's published instructions, starting from fully erased or corrupted application flash. Run that test on the real product — erase it, hand it to someone with a cable and the instructions, and watch. If they can't get it back, the design isn't done.

## 3. The recovery gesture must be reachable — and only deliberate

Recovery entry uses the reset/BOOT/BOOTSEL buttons, which puts this spec in tension with the Button Specification's protection rule — and the resolution is both, not a trade: recovery buttons MUST be actuatable **without opening the case** (a recovery path behind four screws is a depot return with extra steps), MUST be protected from accidental actuation per the Button Specification §7 (recessed, guarded, or off the natural grip — a paperclip-reachable recess is fine; deliberate access is the design goal, disassembly is not), and MUST be labeled (`RST`, `BOOT`) so documented instructions map onto visible hardware. Double-tap timing and gesture conventions stay stock — a vendor who customizes the entry gesture has broken every tutorial, video, and support answer the community has written.

## 4. OTA and self-recovery

Devices that update over the air inherit the same directive at the software layer: an interrupted or failed OTA MUST NOT brick. On ESP32 this is the dual-slot arrangement the Radio & MCU Specification's flash floor exists to guarantee — the old image stays bootable until the new one proves itself, which is why squeezing OTA out of a too-small flash is a recovery regression, not just a feature cut. On nRF52840, BLE OTA through the bootloader carries the same requirement. In all cases the failure mode of a bad update is "boots the previous firmware" or "drops to recovery," never "dark."

## 5. The last-resort layer

Behind the user-facing paths, the board SHOULD expose the platform's debug interface (SWD or JTAG) as labeled test pads — not for users, but for manufacturing programming, failure analysis, and the determined community member with a probe. Pads cost nothing at layout time and are the difference between "interesting failure" and "e-waste" when something genuinely unprecedented happens. On STM32WL designs this layer is promoted to mandatory (§1).

## 6. Duties summary

Almost nothing in this document lives in the variant file — unbrickability is a hardware and documentation property:

| Duty | Requirement |
|---|---|
| Recovery architecture | ROM or write-protected bootloader; entry gesture works with empty/corrupt app flash |
| USB port | Data lines wired; standard tooling (and its auto-reset sequencing) works |
| Acceptance test | Recoverable via the official web flasher from erased flash, by a user with the published instructions |
| Buttons | Recovery buttons reachable without disassembly, protected per Button Spec §7, labeled |
| nRF52 bootloader | Current version shipped; protected region enforced |
| OTA | Failed update falls back or drops to recovery — never dark |
| Debug pads | SWD/JTAG test pads exposed and labeled (mandatory on STM32WL) |
| Documentation | Per-device recovery procedure published, matching the stock gestures |

---

*Companion documents: the LED, Display, Radio & MCU, Button & Switch, Power & Battery, Peripheral Sensor, Antenna & RF, and Sound & Haptics specifications (prescriptive), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
