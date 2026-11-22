# 1.1.3-Bizi (v0.2) — Olimex RP2350PC

Emulator ZX Spectrum (pico-speccy 1.1.3) pentru placa standalone **Olimex RP2350PC** (RP2350B, netlist Rev.A1), HDMI HSTX 640×480@60, flash 4 MB, MinSizeRel, Pico SDK 2.3.1.

**✅ CONFIRMATĂ pe hardware de utilizator (2026-11-21/22): variantă 64fs standard-frame (BCLK 2 MHz, REG02 = 0x10) — test A/B vs v0.1: funcțional.**

## Ce schimbă față de v0.1 (build r6)

- Cadru I2S standard **64-BCLK** (16 date + 16 idle BCLK per half-frame) în loc de 32-BCLK/16-bit-în-32:
  - **BCLK = 2 MHz = 64 × fs** (PIO 4 MHz, LRCK 31250 Hz exact);
  - **ES8311 REG02 = 0x10** (MULT_PRE ×4 → ceas intern 8 MHz = 256 × fs);
  - program PIO dedicat `audio_i2s_lrck_low_64fs` (aceleași pin-uri: DATA=GP31, BCK=GP30, LRCK=GP29).
- Motivație: BCLK 2 MHz evită limita practică „> 1 MHz" din datasheet ES8311 la MULT_PRE ×8 (DVDD 3,3 V) — întrebarea deschisă din v0.1.
- Tot restul configurării = identic cu v0.1.

## Artefacte + SHA-256

- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi-v0.2.uf2` — `ee3f8d2e457947f44dff306e1137b7c4c78ca9bc065ed59e3cdd3bc9a9985335`
- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi-v0.2.bin` — `88c1904d3f18c33f50b9b0481f22465905930bff12d3eccff9ab407ec4710b77`

Surse: tag `v1.1.3-Bizi-v0.2` (commit `92af5b1`). Verificabil prin rebuild bit-exact (aceleași condiții ca v0.1; vezi `FIRMWARE-Bizi-v0.2.md`).

## Flashing

BOOT1 + RESET → **RPI-RP2** → copiați `.uf2`.

Istoric / fallback: v0.1 (`OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.uf2`, SHA `608e5851…`, 32fs non-standard, hw-verificat) rămâne menționat ca referință A/B și opțiune de rollback.

Licență: GPL-3.0 (ca pico-speccy / ESPectrum original).
