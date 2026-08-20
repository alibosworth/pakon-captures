# Capture format

Each `.jsonl` file is one JSON object per line: a `meta` line, then a
time-ordered stream of events. Times (`"t"`) are host wall-clock seconds. Read
the `meta` line first — it declares every stream present and what is omitted.

## Event kinds

| `"d"` | fields | meaning |
|---|---|---|
| `meta` | `label`, `bridge`, `clock`, `scope`, `streams` | one per capture session (a file may hold more than one) |
| `cmd` | `hex` | a PPB command the OEM stack sent |
| `cmd_mod` | `hex` | what actually reached the scanner, when the bridge changed the command (its LED-current safety clamp); absent otherwise |
| `rsp` | `hex` | the scanner's reply |
| `ep0` | `req`, `wValue`, `wIndex`, `dir`, `n` or `blocked` | a USB control transfer; payload omitted. `blocked:true` marks a request the bridge refused (EEPROM write-protection) |
| `ep6` | `n` | one image-data bulk transfer; byte count only, pixels omitted |
| `fw` | `note` | marker: an application-firmware upload happened here; bytes omitted |
| `log` | `msg` | the bridge's own narration, for context |

**Warm vs cold start** is read from the `fw` marker, not a separate flag: a
capture opens before the bridge checks firmware, so a cold start (scanner just
powered on) always writes an `fw` line; its absence means a warm start
(firmware already loaded). Each capture group's README also states which.

## What is omitted, and why it is safe

- **Image pixels** (`ep6`): the film. Timing and volume kept; pixels not.
- **EEPROM contents** (`ep0` payloads): per-unit factory calibration.
- **Firmware** (`fw`): Kodak's copyrighted firmware; only a marker that an upload occurred.

The `scope` field states the boundary: USB application traffic, not the
driver-shim internals or the image-ring mechanics.
