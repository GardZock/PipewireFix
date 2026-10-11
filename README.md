# PipeWire USB Microphone Fix

A focused workaround for a USB microphone capture XRUN observed with PipeWire on one Intel H61 system.

## Quick facts

| Item | Value |
| --- | --- |
| USB microphone | DCMT Technology, vendor/product `31b2:0011` |
| Affected format | ALSA packed `S24_3LE` capture |
| Working format | `S16_LE` in ALSA; `S16LE` in PipeWire properties |
| Changed PipeWire file | `spa/plugins/alsa/alsa-acp-device.c` |
| Patch base | `77b148060acb2dca1da0480c3063d1d536a6a681` |

The patch selects `S16LE` for capture on that USB device only when its ACP profile has not already selected a format. It leaves playback, other devices, and explicit profile formats alone.

## Use the fix

See [HOW_TO_USE.md](HOW_TO_USE.md) for applicability checks, patch application, build, runtime verification, and rollback.

## Project references

- [Patch](patches/fix-usb-mic-s16le.patch)
- [Glossary](GLOSSARY.md)
- [AI context](AI_CONTEXT.md)
- [Structured problem facts](problem.yaml)

## Validation summary

The patch was built and manually exercised with this microphone on Arch Linux, kernel `6.18.55-1-lts`, PipeWire `1.6.9`, and WirePlumber `0.5.18`. Direct ALSA capture with `S24_3LE` failed at startup in the tested combinations; `S16_LE` started and returned frames. PipeWire using the patched ALSA SPA plugin exposed `S16LE` and captured spoken input. This is a hardware-specific result from one device and host controller; automated tests were not run.
