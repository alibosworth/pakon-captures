# Bridge

The captures in this repository are made by a bridge that sits between the
unmodified OEM software and the scanner and logs every command and reply.

`pakonusb.py` (with `ppb.py`) is that bridge, from
[pakon-tlx-macos](https://github.com/pablonavarrob/pakon-tlx-macos) with its
capture-logging patch. The patch is offered upstream as a pull request; a copy
is vendored here so the captures stay reproducible on their own.

Reading the captures needs nothing from this folder — they are plain text. The
code is here only so the capture process can be reproduced. Running the bridge
against real hardware additionally needs `usb1` and the firmware loader from
the full pakon-tlx-macos tree.

Licensed under the upstream project's license: see `LICENSE.upstream`.
