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

Two-button devices place the pair asymmetrically — different positions, ideally different cap shapes — so they are distinguishable by touch. Symmetric button pairs cause wrong-button holds, and a wrong-button 4-second hold is a shutdown.

## 5. Factory reset

The fleet currently has exactly one factory-reset gesture, hard-coded for one board: the ThinkNode M7's screenless build extends the shutdown hold to a 15-second `FACTORY_RESET`. This guide recommends standardizing that gesture for all screenless devices — a screenless node otherwise has no local reset path at all — with two requirements if adopted: the gesture MUST be long enough to be unmistakably deliberate (the M7's 15 seconds is right), and the device MUST give escalating feedback during the hold (buzzer lead-up and/or LED indication) so the user knows they are approaching a destructive action, with a distinct confirmation at the point of no return. Moving this from a per-board special case in `InputBroker.cpp` to a standard screenless behavior is a firmware work item; devices with screens perform factory reset through the UI and need no gesture.

## 6. Optional switches

**Power switch.** Tracker-class and long-deployment devices MAY carry a physical power switch, and it MUST be a true battery disconnect — a slide or rocker that actually breaks the battery circuit, not a GPIO into the firmware. A real disconnect is invisible to the firmware, guarantees zero drain in storage, and works when the firmware is crashed or the battery is too low to boot; a soft "power switch" that depends on firmware cooperation is a button pretending to be a switch and MUST NOT be labeled as power. Slide-style is preferred over toggle precisely because it cannot be pressed accidentally and its state is visible at a glance. (The shutdown *button* semantics remain unchanged — the switch is for storage and transport, not daily power cycling.)

**GPS switch.** Devices with GPS MAY carry a physical GPS switch that cuts power to the GPS module. This is safe by construction: the firmware already controls GPS power on 56 boards (`PIN_GPS_EN`) and probes for the module at startup, degrading gracefully when it finds nothing — so a hardware switch upstream simply makes the node behave as GPS-less. The switch MUST cut the module's power rail, not just its data lines (a powered-but-disconnected GPS drains the battery for nothing), and MUST be labeled. The use case is assured privacy and power discipline: a user who needs GPS *provably* off — beyond trusting a software toggle — gets a switch they can see and feel. The software triple-click toggle remains the everyday path.

Both switches follow the slide-switch ergonomic: state visible, position tactile, impossible to actuate by grip pressure.

## 7. The reset button

A reset button SHOULD be present — it is the recovery path (bootloader entry on ESP32 with BOOT, double-tap UF2 on nRF52), and a device that can't be reset locally can't be rescued locally. But:

**The reset button MUST be difficult to press accidentally.** Too many current devices place reset directly under the user's fingers when the device is held naturally — the result is nodes that reboot mid-use from ordinary grip pressure. On a new design the reset button MUST be recessed, guarded, or otherwise protected: a pinhole actuator, a recess deeper than the cap, a stiff dome under the case wall, or placement on a face the natural grip never loads. Test the claim, not the intent: hold the assembled device every way a user plausibly would — one-handed, pocketed, clipped to a strap — and if any natural hold can actuate reset, the design fails this rule.

Placement rules follow from the same failure mode: reset MUST NOT sit adjacent or symmetric to the user button (a timed-hold gesture performed on the wrong button must not be possible to confuse), and it MUST be labeled `RST` so a deliberate press is easy and an accidental one is hard — that asymmetry is the whole design goal. The same protection standard applies to the ESP32 BOOT button where exposed.

## 8. Declaration checklist

| Hardware present | Variant MUST declare |
|---|---|
| User button | `#define BUTTON_PIN <gpio>` (+ `BUTTON_NEED_PULLUP` if using internal pull-up) |
| Non-standard polarity | `BUTTON_ACTIVE_LOW` / `BUTTON_ACTIVE_PULLUP` overrides |
| Second button, back/alt role | `#define PIN_BUTTON2 <gpio>` (auto-maps to `ALT_BUTTON_PIN`) |
| Second button, cancel role | `#define CANCEL_BUTTON_PIN <gpio>` |
| Capacitive touch pad | `#define BUTTON_PIN_TOUCH <gpio>` (or `PIN_BUTTON_TOUCH`) |
| Power switch (battery disconnect) | Nothing — a true disconnect is invisible to firmware, which is the test that it's real |
| GPS switch | Nothing beyond existing GPS declarations — the firmware's module probe handles the switched-off case |
| Reset / BOOT buttons | Nothing — but the §7 physical-protection rules apply |

As throughout the series: declare exactly what exists. A button define with no button, or a button with no define, fails the audit's dead-define rule.

---

*Companion documents: `meshtastic_led_specification.md`, `meshtastic_display_specification.md`, `meshtastic_radio_mcu_specification.md` (sister prescriptive specs), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
