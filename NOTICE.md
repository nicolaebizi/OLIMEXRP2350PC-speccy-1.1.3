# NOTICE — license provenance for pico-speccy (Olimex RP2350PC port)

Project: this repository is a port of **pico-speccy** ([drewpo28/pico-speccy], a
fork under the GNU General Public License v3) to the **Olimex RP2350PC** board.
Genealogy: [ESPectrum] → [DnCraptor/pico-spec] → [drewpo28/pico-speccy] → this
port. Source modified 2026-11-20 for the Olimex RP2350PC (RP2350B, Rev.A1
netlist). The emulator code carries GPLv3 (`LICENSE` at the repo root).

## ROMs of the machines emulated

### Sinclair / Amstrad ROMs (48K / 128K / +2 / +3 / +3e / +3DIV / ES spanish)
Redistribution of ZX Spectrum ROM images together with emulators is permitted by
**Amstrad plc** for non-commercial use.
> NOTE: the Amstrad statement, published via worldofspectrum.org
> (https://worldofspectrum.org — the official Amstrad permission text), covers
> the 48K, 128K, +2, +3, +3e, +3DIV and the Spanish ES ROMs shipped here.

### Timex TC2048 / TC2068
The Timex ROMs are © Timex Sinclair and are distributed the same way as in the
reference emulators — fuse-emulator/fuse and MAME (image `ts2068_h`/`ts2068_x`).
> NOTE: their presence here mirrors the redistribution those two projects have
> carried for decades.

### ESXDOS 0.8.9 / 0.9.0
© Boriel / Mr J, distributed as freeware by esxdos.org
(https://esxdos.org/index.html).

### TR-DOS 5.03 – 6.11
© Technology Research Ltd. Distributed de facto by the Spectrum community
(UnrealSpeccy, Cip, and similar emulator/ROM sets).
> NOTE: provenance of each TR-DOS variant is recorded in the per-directory
> `manifest.json` (e.g. `src/speccy/roms/pentagon/manifest.json`).

### Russian clone machine ROMs
Pentagon, Scorpion, Profi, Karabas, ATM, TS-Conf (ts-bios, from
tslabs/zx-evo pentevo), Gluk, KAY, Quorum, Navigator, Hobbit, Byte, ALF —
community-sourced firmware dumps; per-variant origin is documented in each
`src/speccy/roms/<machine>/manifest.json`.

### ZX81+ IF2 ROM
Included **by courtesy of Paul Farrow** (explicit attribution to
https://rittwage.com/ipg/?page=if2), as also stated in the README.

## Vendored libraries

- **picomp3lib** — an MIT-licensed wrapper (ikjordan) around the Helix MP3
  decoder core, which is redistributed under the **RPSL / RCSL 1.0**
  (RealNetworks). The original RPSL notes are kept in the directory
  (`external/picomp3lib/LICENSE.txt`, `RCSL.txt`, `RPSL.txt`); the core itself
  is redistributed under the RPSL.
- **embeded-midi-synth** — the fixed-point GM wavetable engine by **xrip**
  (https://github.com/xrip/embeded-midi-synth), included with the **express
  permission of the author**; the grant is documented in
  `external/embeded-midi-synth/NOTICE`. Covered by the project's GPLv3 by
  virtue of that grant.
- **FatFs** and **TJpgDec** — ChaN's BSD-style (1-clause) licenses; the license
  notes are preserved in the source files (`external/fatfs/00readme.txt`,
  `external/tjpgd/tjpgd.c` headers).
- **everything else under `external/`** (tinyusb, Pico-PIO-USB, libxmp, miniz,
  minimp3, dlmalloc; the `sdcard` driver code is vendored under
  `src/drivers/sdcard` on the same terms) — permissive MIT / BSD / CC0 style
  licenses, redistributed with the original notices intact (see the `LICENSE`
  or file headers in each directory).

## Not redistributable — NOT included (instructive)

- **Microsoft `gm.dls` / `gm_bank.bin`** — the General MIDI bank derived from
  Windows' gm.dls is copyrighted and NOT redistributable. It is excluded from
  the repo by `.gitignore` (`gm_bank.bin`, `*.dls`) and the policy is stated in
  `external/embeded-midi-synth/NOTICE`. Users pack their own legally-owned copy
  (`tools/dls_pack.py`) or use the free freepats GUS set.
- **Yamaha YRW801-M** — the General MIDI sample ROM for the ZXM-MoonSound
  (Roland YRW801 samples) is a Yamaha product; legal distributors cannot
  include it in firmware or repositories. Users obtain it themselves and place
  it at `/pico-speccy/roms/yrw801.rom` on the SD card (or build a 16 MB image
  that embeds it for personal use).

## Disclaimer

Nothing in this file (or this repository) constitutes legal advice. If you own
IP included here in error, contact the maintainer for takedown — the model used
is spectrumcomputing.co.uk's own removal practice.

[drewpo28/pico-speccy]: https://github.com/drewpo28/pico-speccy
[ESPectrum]: https://github.com/EremusOne/ESPectrum
[DnCraptor/pico-spec]: https://github.com/DnCraptor/pico-spec
