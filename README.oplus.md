# E404R AnyKernel3 - OPLUS Port variant (dumpcode404 fork, branch `oplus`)

Based on kvsnr113/AnyKernel3 with "drop oplus support" (63db9681d29f) reverted.

## Two zip flavours, one installer
| Zip | Marker file `OPLUS_BUILD` | ROM detected | Result |
|---|---|---|---|
| standard | absent | non-OPLUS | installs |
| standard | absent | OPLUS Port ROM | **aborts** (flash the OPLUS build) |
| OPLUS build | present | OPLUS Port ROM | installs, `e404_args=rom_port,dtbo_oem,...` |
| OPLUS build | present | non-OPLUS | **aborts** |

The OPLUS build kernel is compiled with `CONFIG_OPLUS_PORT=y`, which also spoofs the SELinux
status, so it must not end up on a regular ROM.

OPLUS Port ROM detection: `/vendor/OemPorts10T.prop` or `/vendor/etc/init/OemPorts10T.rc`.
Other OPLUS port projects: `touch /data/local/tmp/e404_force_oplus` before flashing the OPLUS build.

## Fixes over the pre-drop script
* Manual configuration on an OPLUS Port ROM left `rom` empty (`e404_args=,dtbo_oem,...`);
  it now always sends `rom_port`.
* The OPLUS_BUILD marker guards both directions (see table).

CI packs the marker into the zip: `touch AnyKernel3/OPLUS_BUILD` and include it in `zip -r9`.
