# pakon-captures

Faithful USB command/reply captures of Kodak/Pakon F-series film scanners
driven by the original OEM software, for anyone reverse-engineering or
building software for these scanners.

The scanners are long out of support and the community re-derives their
protocol from scratch each time. A shared corpus of real, labelled traffic —
across resolutions, scanner models, and both healthy and faulty units — makes
that work checkable instead of guessed.

## What is here

- **[`captures/`](captures/)** — capture groups, one folder per unit + film +
  session, each with a `README.md` saying exactly what it is. Every file is
  JSON lines; see **[`FORMAT.md`](FORMAT.md)**.
- **[`bridge/`](bridge/)** — the exact bridge code that produced the captures,
  vendored so they are reproducible without any external dependency.
- **[`CAPTURING.md`](CAPTURING.md)** — how captures are made, and how to
  contribute your own.

## What is omitted, always

These captures record protocol traffic, not content. Image pixels, per-unit
EEPROM calibration, and firmware are **never** included — only markers noting
that such a transfer occurred, with its size. Each file's `meta` line declares
this. See [`FORMAT.md`](FORMAT.md).

## Index

| group | unit | film | configs | date |
|---|---|---|---|---|
| [f135plus-serial16402-4expneg-20260820](captures/f135plus-serial16402-4expneg-20260820/) | F-135+ (16402) | 4-exp C-41 negative | Base 4/8/16 × ICE on/off, + 4 extras | 2026-08-20 |

## Related

- [pakon-reference](https://github.com/alibosworth/pakon-reference) — the
  protocol facts these captures are evidence for.
- [pakon-tlx-macos](https://github.com/pablonavarrob/pakon-tlx-macos) — the
  bridge (Wine + userspace driver) whose capture mode produced these.

## Contributing

Other units, other film, long rolls, a working DX sensor — all wanted. See
[`CAPTURING.md`](CAPTURING.md).

## License

The captures are released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
The vendored `bridge/` code is under its upstream license (see `bridge/`).
