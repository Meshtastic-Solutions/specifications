# Meshtastic Device Button & Switch Specification

**Status: Draft for discussion** · Fourth prescriptive document in the device design guide series (with the LED, Display, and Radio & MCU specifications) · Derived from the firmware device-behavior audit (master + develop, July 2026)

This document prescribes button and switch hardware for new Meshtastic devices. The firmware's button semantics are already uniform and well-defined — the behavior guide documents them — so this specification concerns the hardware side: which controls a device must have, how they are wired and declared, and how they are physically protected. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. The user button

**Every user-facing device MUST have a physical user button.** 147 of the 176 current variants have one; among the 24 that don't are devices with no local path to shutdown, position ping, or GPS toggle at all — functions the firmware provides for free the moment a button exists. Headless infrastructure nodes (rack routers, embedded modules) are the only legitimate exception.

The button is momentary and tactile: the user should feel the actuation, including through gloves, and be able to find it by touch — the button semantics include timed holds (0.5–4 s select, past ~4 s shutdown), which are unusable on a mushy or ambiguous switch. The firmware supplies all behavior (click, double, triple, hold tiers, per the behavior guide); the hardware's only job is a clean, unambiguous press.

Touchscreens, capacitive pads, and keyboards supplement the user button; they never replace it. This restates the Display Specification's rule from the hardware side: wake, shutdown, and recovery must survive a crashed UI, a cracked digitizer, and winter gloves.

## 2. Mechanical robustness

**Buttons MUST be as mechanically robust as possible.** Buttons detaching from the PCB is a recurring, documented failure across shipped Meshtastic devices, and it is a total failure: per §1, the button is the wake, shutdown, and recovery path, so a sheared-off switch strands the device. The engineering principle is simple — **press force and prying force must never be carried by solder joints alone**:

The switch MUST be mechanically anchored beyond its electrical pads: through-hole mounting, an SMD part with locating pegs seated in PCB holes, or a metal-frame switch with dedicated anchor tabs soldered to the board. A bare pad-mount tactile switch with nothing else holding it is the exact part that ships in year one and detaches in year two.

The enclosure SHOULD carry the load. The best arrangement is a case-mounted actuator (a cap or plunger in the enclosure wall) pressing a board-mounted switch: the user's finger touches the case, the case constrains the force to pure perpendicular travel with an over-travel stop, and lateral or prying loads go into the enclosure instead of the part. A switch actuated directly through a case cutout invites exactly the off-axis loads that peel pads, and side-actuated switches hanging on a board edge MUST be braced by the enclosure or not used.

Timed holds raise the stakes: the firmware's semantics have users pressing continuously for 4 to 15 seconds, often with the thumb loading the switch at whatever angle the grip dictates — a duty cycle bare tactile switches on pads were never meant to carry. Test accordingly: rated actuations are necessary but not sufficient; the assembled device MUST survive off-axis presses, fingernail actuation at the cap edge, and sustained full-force holds without the switch loosening. The §7 reset-button protections and this section are two halves of the same discipline — reset is guarded from *accidental* force, and every button is built for *deliberate* force applied badly.

## 3. Wiring and declaration

**Active-low with a pull-up is the standard wiring**: the button connects the GPIO to ground, and the line idles high. This is the firmware default (`BUTTON_ACTIVE_LOW` / `BUTTON_ACTIVE_PULLUP` both default true) and the fleet's near-unanimous practice — only 5 of 147 button-bearing boards wire active-high, all sealed-membrane designs. Use the MCU's internal pull-up and declare `BUTTON_NEED_PULLUP` where the platform requires it (48 boards do today); an external pull-up MAY be used where the pin lacks an internal one. A non-standard polarity MUST be declared (`BUTTON_ACTIVE_LOW false` etc.) — the same omitted-polarity bug class the LED Specification closes applies here.

The variant declares `BUTTON_PIN` for the user button. Note that users can remap this GPIO at runtime (`device.button_gpio`, provided as a broken-button escape hatch), so the declared pin is the default, not a guarantee — one more reason no *other* function should share it.

Debounce is the firmware's job; no hardware debounce network is required, and hardware latching or toggle circuits on the user button MUST NOT be used — the timing tiers assume a direct momentary contact.

## 4. The second button

**Devices with a screen SHOULD have a second button** in the back/cancel position; 26 boards ship two-button layouts today and the firmware carries complete semantics for both roles. The standard second-button role is **alt/back** (`PIN_BUTTON2`, auto-mapped to `ALT_BUTTON_PIN`): click for back/next, half-second hold for the long-alt action. The alternative role is **cancel** (`CANCEL_BUTTON_PIN`): click dismisses, 4-second hold shuts down — appropriate where the designer wants a second shutdown path or a dismiss-first interaction. A variant chooses one role per button; declaring `CANCEL_BUTTON_PIN` suppresses the alt auto-mapping, so the choice is explicit in the variant file.

On PMU boards the power button can carry the cancel role directly (§6), which may remove the need for a discrete second button where cancel — rather than alt/back — is what the design wants. Note the roles are not identical: the PMU button yields `CANCEL` on a short click, where a `CANCEL_BUTTON_PIN` also carries the 4-second hold to shutdown.

Two-button devices place the pair asymmetrically — different positions, ideally different cap shapes — so they are distinguishable by touch. Symmetric button pairs cause wrong-button holds, and a wrong-button 4-second hold is a shutdown.

## 5. Factory reset

The fleet currently has exactly one factory-reset gesture, hard-coded for one board: the ThinkNode M7's screenless build extends the shutdown hold to a 15-second `FACTORY_RESET`. This guide recommends standardizing that gesture for all screenless devices — a screenless node otherwise has no local reset path at all — with two requirements if adopted: the gesture MUST be long enough to be unmistakably deliberate (the M7's 15 seconds is right), and the device MUST give escalating feedback during the hold (buzzer lead-up and/or LED indication) so the user knows they are approaching a destructive action, with a distinct confirmation at the point of no return. Moving this from a per-board special case in `InputBroker.cpp` to a standard screenless behavior is a firmware work item; devices with screens perform factory reset through the UI and need no gesture.

## 6. Power and GPS controls

**Power: a PMU power button, not a cutoff switch.** New designs SHOULD NOT carry a hard power-cutoff switch in the battery circuit. Breaking that circuit at an arbitrary instant risks corrupting the filesystem — the firmware persists the node database and configuration in the background, and a disconnect landing mid-write costs the user their settings or their node list. That failure presents as a device that quietly "forgot everything," which is far harder for a user to diagnose than a flat battery, and it is self-inflicted: the device was working when they flipped the switch.

The preferred arrangement is a **PMU power button** — the AXP arrangement on the T-Beam family, where the button is wired to the power-management IC rather than straight across the cell. Because the firmware can see the press, shutdown is orderly: writes are flushed and the radio brought down before power drops. The PMU's own hardware long-press sits underneath as a last-resort cutoff for a wedged device, which is the case a cutoff switch was really there for — so the arrangement keeps the guarantee and drops the corruption risk.

It also earns a second control for free. With `PMU_POWER_BUTTON_IS_CANCEL`, a short click of the power button emits `CANCEL` — in practice, sleeping the screen — giving the device a dedicated cancel control with no additional part, panel cutout, or seal to fail. Three boards ship this today (T-Beam, T-Beam S3, T-Watch S3). It is worth reading alongside §4, which otherwise spends an entire second button on that role.

Where a true disconnect is genuinely required — shipping regulations, or storage measured in months — it MUST be labeled and positioned as a storage disconnect, to be operated only after the device has been shut down, and MUST NOT be presented as the everyday power control.

**GPS switch.** Devices with GPS MAY carry a physical GPS switch that cuts power to the GPS module. This is safe by construction: the firmware already controls GPS power on 56 boards (`PIN_GPS_EN`) and probes for the module at startup, degrading gracefully when it finds nothing — so a hardware switch upstream simply makes the node behave as GPS-less. The switch MUST cut the module's power rail, not just its data lines (a powered-but-disconnected GPS drains the battery for nothing), and MUST be labeled. The use case is assured privacy and power discipline: a user who needs GPS *provably* off — beyond trusting a software toggle — gets a switch they can see and feel. The software triple-click toggle remains the everyday path.

The GPS switch — and any storage disconnect fitted per above — follows the slide-switch ergonomic: state visible, position tactile, impossible to actuate by grip pressure.

## 7. The reset button

A reset button SHOULD be present — it is the recovery path (bootloader entry on ESP32 with BOOT, double-tap UF2 on nRF52), and a device that can't be reset locally can't be rescued locally. But:

**The reset button MUST be difficult to press accidentally.** Too many current devices place reset directly under the user's fingers when the device is held naturally — the result is nodes that reboot mid-use from ordinary grip pressure. On a new design the reset button MUST be recessed, guarded, or otherwise protected: a pinhole actuator, a recess deeper than the cap, a stiff dome under the case wall, or placement on a face the natural grip never loads. Test the claim, not the intent: hold the assembled device every way a user plausibly would — one-handed, pocketed, clipped to a strap — and if any natural hold can actuate reset, the design fails this rule.

Placement rules follow from the same failure mode: reset MUST NOT sit adjacent or symmetric to the user button (a timed-hold gesture performed on the wrong button must not be possible to confuse), and it MUST be labeled `RST` so a deliberate press is easy and an accidental one is hard — that asymmetry is the whole design goal. The same protection standard applies to the ESP32 BOOT button where exposed.

## 8. Controls behind an I2C GPIO expander

Pin-constrained designs MAY put controls and indicators behind an I2C GPIO expander, and three boards do today: the ELECROW ThinkNode M5 drives LEDs through a PCA expander (`PCA_LED_*`), the T5 S3 e-paper board gates a capacitive home button through a PCA9535 (`BOARD_PCA9535_BUTTON_MASK`), and the SenseCAP Indicator routes its touch controller's interrupt and reset through one (`SCREEN_TOUCH_INT`/`_RST` with an `IO_EXPANDER` flag). The expander is a legitimate tool. Using it without an interrupt line is not.

**An expander carrying any input MUST have its interrupt output wired to a real MCU GPIO, and that GPIO MUST be able to wake the MCU from the sleep states the design uses.** An expander with no `INT` connection can only be discovered by polling it over I2C, and polling costs three things that matter here:

**Wake.** A sleeping MCU cannot poll. An input whose only path to the firmware is an I2C read is an input that does nothing in precisely the state the user presses it from — the device is asleep, which is why they are pressing the button. The fleet already shows where this lands: on the T5 S3 e-paper board, touch is deliberately excluded as a light-sleep wake source. A button that cannot wake the device is not a button, it is a button-shaped part.

**Timing.** The semantics in §1 are built on timing: a roughly quarter-second multi-click window, a 0.5–4 s select band, a hold past ~4 s for shutdown. A poll loop adds its interval as jitter to both edges of every press, and an interval long enough to be power-sane is long enough to break multi-click detection outright. Interrupt-driven edges give the debouncer honest timestamps; polled edges give it noise.

**Power.** Waking an I2C bus on a schedule to learn that nothing happened is textbook avoidable draw on a node expected to run for months.

Wiring and handling follow from that. The expander's `INT` is typically open-drain active-low and needs a pull-up — the MCU's internal pull-up is fine where the pin has one. Where several expanders share a bus, their interrupt outputs MAY be wire-OR'd onto one GPIO, since the handler reads each expander's status registers regardless. The design MUST allow firmware to determine which pin changed and to clear the interrupt by reading the expander; a part that latches in a way firmware cannot clear, or that cannot report the source pin, forces the poll loop back again and fails this section. The expander MUST NOT sit behind a bus switch or on a rail that firmware powers down while an input on it is still expected to wake the device.

Some lines MUST NOT be placed on an expander at all:

- **The radio's IRQ, `BUSY`, and reset lines** — see the Radio & MCU Specification, which treats this as a hard requirement for timing reasons.
- **Reset and BOOT** (§7). Recovery must work with no application firmware present; a ROM bootloader knows nothing about an I2C expander, so a recovery gesture behind one is not a recovery gesture. This is the Recovery & Flashing Specification's prime directive in hardware form.
- **The primary user button** SHOULD be on a real GPIO. It is the wake, shutdown, and recovery path of §1, users can remap it at runtime (`device.button_gpio`) to a real GPIO when it breaks, and it carries the tightest timing of any control on the board. The expander is for secondary controls — a second button, a notification LED, a backlight enable — not for the one control the device cannot do without.

## 9. Declaration checklist

| Hardware present | Variant MUST declare |
|---|---|
| User button | `#define BUTTON_PIN <gpio>` (+ `BUTTON_NEED_PULLUP` if using internal pull-up) |
| Non-standard polarity | `BUTTON_ACTIVE_LOW` / `BUTTON_ACTIVE_PULLUP` overrides |
| Second button, back/alt role | `#define PIN_BUTTON2 <gpio>` (auto-maps to `ALT_BUTTON_PIN`) |
| Second button, cancel role | `#define CANCEL_BUTTON_PIN <gpio>` |
| Capacitive touch pad | `#define BUTTON_PIN_TOUCH <gpio>` (or `PIN_BUTTON_TOUCH`) |
| PMU power button (preferred over a cutoff switch) | `HAS_AXP192` / `HAS_AXP2101` (derives `HAS_PMU`), plus `PMU_POWER_BUTTON_IS_CANCEL` for the cancel / screen-sleep role |
| Storage disconnect, where one is fitted | Nothing — a true disconnect is invisible to firmware |
| GPS switch | Nothing beyond existing GPS declarations — the firmware's module probe handles the switched-off case |
| Reset / BOOT buttons | Nothing — but the §7 physical-protection rules apply, and §8 forbids placing them behind an expander |
| Control or indicator behind an I2C GPIO expander | The expander part and address, the mapped pin, and the MCU GPIO its `INT` output is wired to (§8) |
| Expander `INT` line | A wake-capable GPIO, with a pull-up where the pin has none |

As throughout the series: declare exactly what exists. A button define with no button, or a button with no define, fails the audit's dead-define rule.

---

*Companion documents: `meshtastic_led_specification.md`, `meshtastic_display_specification.md`, `meshtastic_radio_mcu_specification.md` (sister prescriptive specs), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
