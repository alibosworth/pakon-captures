# F-135+ (serial 16402), 4-exposure C-41 negative strip, 2026-08-20

Six PPB command/reply captures of the unmodified OEM software stack driving a
Kodak/Pakon F-135+, one per scan resolution and Digital ICE state, plus four
"extras" recording failures and quirks from the same session.

## The unit and the session

| | |
|---|---|
| Scanner | Pakon F-135+, serial 16402 |
| Firmware (ROM versions) | USB 0x03,0x0F · CCD 0x00,0x00 · Lamp 0x05,0x0A · DX 0x00,0x00 · Motor 0x05,0x06 · APS 0x00,0x00 |
| Film | one 4-exposure C-41 colour-negative strip, reused for every scan |
| Date | 2026-08-20 |
| Capture tool | the [vendored bridge](../../bridge/) (pakon-tlx-macos + capture logging) |
| Contributor | Ali Bosworth |

**This unit's DX sensor is faulty**: it does not decode edge barcodes, so the
OEM software labels every frame `DX_Error`. The DX command exchange is still
present and real in every capture — the scanner reports no code, and these
captures show exactly what that looks like. A capture from a unit with a
working DX sensor would be a valuable addition.

The scanner was powered continuously across all scans (warm firmware, no
upload, so no `fw` marker), and the OEM client was restarted between scans, so
each file is a self-contained scan.

## The six configurations

The scan trigger `0x91` (SetScanLineParams) carries a 16-bit little-endian
value that identifies the configuration; it appears twice per scan
(calibration trigger, then transport trigger). The motor rate (`WRITE PICM
MOTOR RATE`) is a second, independent identifier.

| file | resolution | Digital ICE | 0x91 | motor rate | scan | image transfers |
|---|---|---|---|---|---|---|
| `base4.jsonl` | Base 4 | off | 0x0107 | 0x647e | 48 s | 4234 |
| `base4-ir.jsonl` | Base 4 | on | 0x00c5 | 0x4b4e | 44 s | 5122 |
| `base8.jsonl` | Base 8 | off | 0x0075 | 0x2caa | 50 s | 6582 |
| `base8-ir.jsonl` | Base 8 | on | 0x004d | 0x1d85 | 82 s | 8074 |
| `base16.jsonl` | Base 16 | off | 0x003c | 0x170c | 71 s | 10135 |
| `base16-ir.jsonl` | Base 16 | on | 0x0031 | 0x12e4 | 85 s | 13314 |

Three of the `0x91` values (Base 4, Base 8, Base 16 without ICE) match an
independent OEM Windows datalogger capture of the same unit byte for byte.

## extras/

Not clean scans; kept because failure and edge behaviour is hard to capture
deliberately.

| file | what it is |
|---|---|
| `base8-scanerror.jsonl` | a live `WTO_SCANERROR` / `EC_DRV_LostSync` incident: the transport trigger, then `link lost: short read` |
| `base16-scanerror.jsonl` | a second such incident at Base 16 |
| `base8-ir-latefeed.jsonl` | Base 8 + ICE, but the film was fed ~45 s late: the engine's behaviour while waiting for film |
| `base16-ir-nofeed.jsonl` | Base 16 + ICE with no film fed, plus an aborted start; two sessions (two `meta` lines) in one file |

## Format

See [`../../FORMAT.md`](../../FORMAT.md). Each file is JSON lines; the first
line (`"d":"meta"`) declares the streams and what is omitted (image pixels,
EEPROM contents, firmware).
