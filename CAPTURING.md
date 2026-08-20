# How to make a capture

These captures are produced by a man-in-the-middle bridge between the
unmodified OEM software and the scanner: on macOS/Linux the OEM Windows client
runs under Wine and its kernel driver is replaced by a userspace shim that
talks libusb. The bridge sees every command and reply and writes them out.

The bridge is [pakon-tlx-macos](https://github.com/pablonavarrob/pakon-tlx-macos)
(by Pablo Navarro). The capture mode used here is offered upstream as a pull
request; until it is part of a release, the exact bridge that made the
captures in this repository is vendored in [`bridge/`](bridge/) so they remain
reproducible independently.

## Steps

1. Run the OEM stack through the bridge per pakon-tlx-macos's own setup
   (`./run.sh`).

2. Enable capture by pointing an environment variable at an output file
   **before starting the server**, and label the configuration:

   ```sh
   export PAKON_CAPTURE=~/captures/<name>.jsonl
   export PAKON_CAPTURE_LABEL=<name>          # stamped into the meta line
   ./run.sh stop && ./run.sh
   ```

   One server start per capture keeps one file per scan. The bridge also
   keeps a dated copy of its own text log per session.

3. In the OEM client, set the scan configuration (resolution, Digital ICE),
   and scan. Record what you set — the client's own settings are the ground
   truth for the label; the `0x91` payload in the capture confirms it.

4. Stop the transport fully before the next capture.

## What a good capture needs

- **State the configuration at collection time.** The file name and the
  `PAKON_CAPTURE_LABEL` are the label; put the full description (unit, serial,
  firmware, film, date, ICE state) in the group's `README.md`.
- **Run the bridge in plain pass-through.** For a faithful OEM corpus, do not
  run any tool that injects or rewrites traffic; the capture should be the
  OEM software and the scanner talking, with the bridge only observing (and
  applying its hardware-safety clamp, which is recorded as `cmd_mod` on the
  rare command it touches).
- **Warm vs cold start matters.** A capture from scanner power-on includes the
  firmware upload (recorded only as a `fw` marker, bytes omitted) and the
  enumeration. Note which you did.

## Contributing

Captures from other units, other film, other scanner models, or working DX
sensors are all wanted. Add a folder under `captures/` named for what it is —
model, serial, film, date, e.g. `f135-serialNNNNN-longroll-YYYYMMDD/` — with a
`README.md` describing the unit and session, following the format in
[`FORMAT.md`](FORMAT.md). Confirm your files carry no omitted-data payloads
(no image pixels, EEPROM contents, or firmware bytes); the bridge's capture
mode already enforces this, but check if you used other tooling.
