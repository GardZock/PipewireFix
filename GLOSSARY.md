# Glossary

| Term | Meaning here |
| --- | --- |
| **ALSA** | Linux's low-level audio interface used by the kernel and applications. PipeWire uses ALSA to access this microphone. |
| **ACP** | ALSA Card Profile: PipeWire's profile and device handling layer for ALSA sound cards. |
| **Capture** | Audio flowing from the microphone into an application. Also called recording or input. |
| **EPIPE / `-32`** | ALSA's error code for a broken PCM stream. In this reproduction, it appears when capture starts in `S24_3LE`. |
| **Format** | How each audio sample is represented, including its bit depth and byte layout. |
| **PCM** | Pulse-code modulation: the stream of digital audio samples carried by the device. |
| **PipeWire** | The Linux multimedia service that connects audio/video devices to applications. |
| **SPA** | Simple Plugin API, the plugin interface used by PipeWire components such as its ALSA driver. |
| **S16_LE / S16LE** | Signed 16-bit audio samples stored little-endian. ALSA and PipeWire use slightly different names for the same format. |
| **S24_3LE** | Signed 24-bit samples packed into three little-endian bytes. The tested microphone advertises it, but its capture stream XRUNs with this format on the tested host. |
| **USB VID:PID** | USB vendor ID and product ID. `31b2:0011` identifies the tested microphone model in this report. |
| **XRUN** | An ALSA overrun or underrun state. For microphone capture, it means the PCM stream failed to keep running and capture stops. |
