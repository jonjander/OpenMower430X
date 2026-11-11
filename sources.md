# Sources

Checked against the OpenMower documentation on 2026-10-16, and against this mower when a page says so.

Start at [openmower.de/latest/docs/](https://openmower.de/latest/docs/). English and German share the page tree. The [links page](https://openmower.de/latest/docs/links/) is partly out of date. The current map of repositories is the ecosystem table in the [OpenMower README](https://github.com/ClemensElflein/OpenMower/blob/main/README.md). [wiki.openmower.de](https://wiki.openmower.de/) still describes the project as YardForce 500 in its FAQ. Do not use it as a current source.

Picture guides exist only for YardForce and for SABO / John Deere. Under the Universal board, the documentation says most Husqvarna mowers are likely to fit and sends the exact model to Discord. Nothing there describes a finished 430X conversion. `Husq310MKII` in the firmware is the 310 MKII, not the 430X.

The hardware on this build is the current module set, not the retired universal board:

- [hw-module-logic](https://github.com/xtech/hw-module-logic)
- [hw-module-xesc](https://github.com/xtech/hw-module-xesc)
- [hw-module-charger](https://github.com/xtech/hw-module-charger)
- [hw-module-gps](https://github.com/xtech/hw-module-gps)

Firmware behavior cited in these pages is `fw-openmower-v2` tag v1.4.3. The OS image is OpenMowerOS v2.1.1. The computer is a Raspberry Pi Compute Module 5.

The Swedish working journal, including measurements that are not settled enough for a page here, is in [notes/](../notes/).
