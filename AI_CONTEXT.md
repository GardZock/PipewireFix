# AI context

## Canonical facts

- This project contains one focused PipeWire source patch and instructions for applying it.
- The patch changes `spa/plugins/alsa/alsa-acp-device.c` in PipeWire.
- The tested device is a DCMT Technology USB Condenser Microphone, USB vendor/product ID `31b2:0011`, on an Intel H61 system.
- Direct ALSA capture on the tested host failed at startup with `S24_3LE` (`-EPIPE` / XRUN) and worked with `S16_LE`.
- The patch supplies `S16LE` for this device's capture node only when the ACP profile has no explicit format.
- A locally rebuilt PipeWire exposed the source as `S16LE`; spoken capture was confirmed on the tested machine.
- The patch was built against PipeWire commit `77b148060acb2dca1da0480c3063d1d536a6a681`.

## Limits on conclusions

- Treat this as a device-specific workaround validated on one microphone and host-controller combination.
- Do not claim that every H61 board, USB microphone, PipeWire version, or `S24_3LE` stream has this failure.
- Manual hardware checks were performed; automated tests were not run.
- Do not imply that ALSA direct capture and PipeWire capture exercise the same software path. The ALSA comparison isolates the device format behavior; the PipeWire check validates the patched runtime path.

## When editing this repository

1. Read `README.md`, `HOW_TO_USE.md`, `GLOSSARY.md`, and `problem.yaml`.
2. Keep the patch narrow: capture direction, USB ID `31b2:0011`, and no override when the ACP profile already sets a format.
3. Keep the patch tied to its stated PipeWire base revision. If the base changes, check the patch against the new source and update the revision and usage instructions.
4. Separate observed results from explanations or hypotheses in all generated text.
