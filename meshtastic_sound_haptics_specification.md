# Meshtastic Device Sound & Haptics Specification

**Status: Draft for discussion** · Eighth prescriptive document in the device design guide series (with the LED, Display, Radio & MCU, Button & Switch, Power & Battery, Peripheral Sensor, and Antenna & RF specifications) · Derived from the firmware device-behavior audit (master + develop, July 2026)

This document prescribes audio and haptic hardware for new Meshtastic devices. Sound and vibration are the notification channels that work when the user isn't looking at the device — which, for a radio in a pocket or on a pack strap, is nearly always. They are also the third leg of the accessibility triad this series has been building: the LED Specification made the visual channel colorblind-safe, and this document makes the audible and tactile channels real. The key words MUST, MUST NOT, SHOULD, and MAY are used in the RFC 2119 sense.

## 1. The buzzer — and the active buzzer problem

**User-facing devices SHOULD have a buzzer, and it MUST be a passive transducer. Active buzzers MUST NOT be used.**

The distinction matters because of what the firmware plays. Meshtastic's audio is *frequency content*: RTTTL ringtones the user selects, boot and shutdown melodies, keyboard and touch feedback clicks, and — most functionally — the escalating lead-up chirp during a long button hold, whose *rising pitch* is the warning that a shutdown is approaching. A passive buzzer is a transducer: the firmware's PWM drive sets the pitch, and all of that content works. An active buzzer contains its own oscillator and plays exactly one note whenever powered: every ringtone collapses into the same flat drone, pitch-coded cues lose the information they carry, and PWM into an internal oscillator produces the broken-sounding buzz that generates "my buzzer sounds wrong" support threads. The parts look alike and cost alike, which is exactly why this rule needs stating: **specify the passive part, and verify audibly on the first prototype** — if every tone sounds identical, the wrong part is on the board.

Fifty-five boards ship buzzers today, so the wiring conventions are established: declare `PIN_BUZZER`, and where the buzzer sits behind a switched rail, declare `BUZZER_EN_PIN` (four boards do) so the firmware can silence the rail for sleep.

**Let the sound out.** The Sensor Specification's sealed-case honesty rule has an acoustic twin: a buzzer behind an unported case wall is muffled into uselessness, and an outdoor-alert device that can't be heard outdoors failed at the mechanical layer. Give the buzzer a sound port; for weather-resistant designs, port it through an acoustic membrane (standard practice in sealed consumer devices) rather than sealing it silent. If the enclosure genuinely cannot be ported, measure and document the real loudness — don't ship a "loud alert" feature the case has already vetoed.

## 2. I2S audio

**I2S audio is supported and integrated, and speaker-equipped designs are encouraged.** The firmware's tone path is unified: system tones and ringtones are generated as RTTTL and rendered through an audio abstraction, so on an I2S board the same content that drives a buzzer plays through the codec and speaker — a speaker is a strict upgrade, not a parallel system, and an I2S design needs no separate buzzer. Eight boards ship I2S audio today (the T-Deck class, the TLora Pager's ES8311 codec, MAX98357-amp designs), and ESP32-class I2S boards additionally unlock the voice module (codec2 audio over the mesh) — a genuinely differentiating feature for a communication device.

Two rules keep speakers battery-honest: the amplifier MUST have a firmware-controllable enable/mute (declared in the variant) so sleep actually silences it — an idle amplifier is standing current plus hiss — and the I2S pin set and codec MUST be fully declared, since audio is configured per-variant, not auto-detected.

## 3. Haptics

**A vibration motor is RECOMMENDED for pocket-carried and wearable devices.** The firmware support is already complete — the External Notification module drives `PIN_VIBRATION` with a half-second buzz per message and a nag cycle until acknowledged — and vibration is the only notification channel that works in *both* directions of the volume problem: the loud environment where the buzzer is inaudible, and the silent one where it's unwelcome. It's also the channel that serves deaf and hard-of-hearing users, completing the triad: LED for the eyes, buzzer for the ears, vibration for the hand. Only one board ships it today (T-Deck Pro), which makes this less a fleet convention than an open opportunity — the firmware is waiting for the hardware.

Touch-equipped devices MAY add a haptic driver for touch feedback (`HAPTIC_FEEDBACK_PIN`, precedent on the capacitive-button boards: a blip on touch and a second at the long-press threshold — tactile confirmation for a control that otherwise gives none).

One mechanical note, inherited from the Button Specification's robustness rule: vibration motors are a moving mass whose entire job is shaking their own mounting. A coin motor held by solder pads and hope works loose; anchor it mechanically (adhesive pad, enclosure pocket, or bracket) so the feature doesn't uninstall itself.

## 4. The user owns silence

The firmware gives the user a five-position buzzer policy — all sounds, disabled, notifications only, system sounds only, or direct-messages only — plus full External Notification control over ringtones, nagging, and vibration. That control is only real if the hardware routes every sound through the firmware: **a device MUST NOT make sound the firmware cannot silence.** No hardware-driven beepers, no charger chimes, no tones wired outside firmware control — this is the acoustic mirror of the LED Specification's no-charger-LED rule, and the reasoning is identical: one authority per channel, and the user's settings govern it. A device that beeps against its owner's configuration is a device that gets left at home.

## 5. Declaration checklist

| Hardware present | Variant MUST declare |
|---|---|
| Passive buzzer | `#define PIN_BUZZER <gpio>` |
| Buzzer on a switched rail | `#define BUZZER_EN_PIN <gpio>` |
| I2S codec / amplifier / speaker | Full I2S pin set, codec selection, and the amp enable/mute control |
| Vibration motor | `#define PIN_VIBRATION <gpio>` |
| Touch haptic driver | `#define HAPTIC_FEEDBACK_PIN <gpio>` |

As throughout the series: declare exactly what exists — and for this document specifically, verify the buzzer is the passive part before the variant claims `PIN_BUZZER` at all.

---

*Companion documents: `meshtastic_led_specification.md`, `meshtastic_display_specification.md`, `meshtastic_radio_mcu_specification.md`, `meshtastic_button_specification.md`, `meshtastic_power_battery_specification.md`, `meshtastic_sensor_specification.md`, `meshtastic_antenna_rf_specification.md` (sister prescriptive specs), `meshtastic_standard_behavior_guide.md` (descriptive), and the device-behavior audit (evidence base). These documents are intended for eventual collection into a unified Meshtastic device design guide.*
