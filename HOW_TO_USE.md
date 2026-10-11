# How to use the fix

## Check that the fix matches your device

This patch is for the DCMT Technology USB Condenser Microphone with USB ID `31b2:0011`. Check the ID before applying it:

```sh
lsusb -d 31b2:0011
```

The workaround is limited to this device's **capture** path. It selects `S16LE` only when the ALSA ACP profile has not already chosen a format.

## Apply and build

The patch targets PipeWire commit `77b148060acb2dca1da0480c3063d1d536a6a681`. Clone both repositories into the same parent directory, then apply the patch:

```sh
git clone https://gitlab.freedesktop.org/pipewire/pipewire.git
git clone https://github.com/GardZock/PipewireFix.git

cd pipewire
git checkout 77b148060acb2dca1da0480c3063d1d536a6a681
git apply --check ../PipewireFix/patches/fix-usb-mic-s16le.patch
git apply ../PipewireFix/patches/fix-usb-mic-s16le.patch
```

Build the ALSA SPA plugin:

```sh
meson setup build
ninja -C build spa/plugins/alsa/libspa-alsa.so
```

If `build/` is already configured, skip `meson setup`. PipeWire's [build guide](https://gitlab.freedesktop.org/pipewire/pipewire/-/blob/master/INSTALL.md) lists build dependencies and options.

## Run and verify

For a development run without installing over the distribution's PipeWire:

1. Stop the currently running user PipeWire services and session manager so the development daemon can use its sockets. On a systemd desktop, the unit names are usually `pipewire`, `pipewire-pulse`, and `wireplumber`.
2. In the PipeWire source directory, enter the uninstalled build environment:

   ```sh
   ./pw-uninstalled.sh -b build
   ```

3. In the shell opened by that command, start the build-tree daemon:

   ```sh
   cd build
   make run
   ```

This follows the **Running** section of PipeWire's [build guide](https://gitlab.freedesktop.org/pipewire/pipewire/-/blob/master/INSTALL.md). The uninstalled environment points `SPA_PLUGIN_DIR` at the build-tree plugins, so the rebuilt ALSA SPA plugin is used.

In a second terminal, enter the same uninstalled environment:

```sh
cd /path/to/pipewire
./pw-uninstalled.sh -b build
```

After the development shell opens, record a short sample while speaking:

```sh
pw-record --target alsa_input.usb-DCMT_Technology_USB_Condenser_Microphone-00.mono-fallback \
  --rate 48000 --channels 1 /tmp/pipewire-mic-check.wav
```

Stop with `Ctrl+C`, then play `/tmp/pipewire-mic-check.wav`. The expected result is audible voice capture, with the source using `S16LE` and no immediate capture XRUN. The file is created locally for this check; it is not part of this repository. Afterward, stop the development daemon and restart the user services that were stopped in step 1.

The earlier validation used PipeWire `1.6.9`, WirePlumber `0.5.18`, Linux `6.18.55-1-lts`, and this microphone connected to an Intel H61 USB controller. Other systems and device revisions need their own verification.

## Undo the source change

From the PipeWire source directory:

```sh
git apply --reverse ../PipewireFix/patches/fix-usb-mic-s16le.patch
```

If you used PipeWire's development-run environment, stop that run and restart the user services you stopped before testing.
