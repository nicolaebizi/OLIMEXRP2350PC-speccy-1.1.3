# Firmware 1.1.3-Bizi (v0.2) — hw-confirmed — Olimex RP2350PC

- **Baza:** build-rp2350pc-r7, configurație **MinSizeRel**, target `RP2350PC`
  (`-DRP2350PC=ON -DHDMI_HSTX=TMDS`), Pico SDK 2.3.1, amprentă flash 4 MB.
- **Varianta 64fs (r7):** cadru I2S standard **64-BCLK** (16 date + 16 idle BCLK
  per half-frame), **BCLK = 2 MHz = 64 × fs**, LRCK 31250 Hz exact (PIO 4 MHz);
  ES8311 **REG02 = 0x10** (MULT_PRE ×4 → ceas intern 8 MHz = 256 × fs).
  Restul configurării = identic cu v0.1 (pinii I2S GP31/30/29, codec I2C 0x18,
  fără MCLK extern).
- **✅ CONFIRMAT pe hardware de utilizator (2026-11-21/22):** test A/B vs v0.1 —
  funcțional. BCLK 2 MHz evită limita practică „> 1 MHz" a datasheet-ului
  ES8311 la MULT_PRE ×8.
  Referință istorică / rollback: v0.1 (uf2 SHA `608e5851…`, 32fs non-standard,
  hw-verificat).

## Fișiere / Files

- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi-v0.2.uf2` — imagine flash
- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi-v0.2.bin` — binar brut

SHA-256:

```
ee3f8d2e457947f44dff306e1137b7c4c78ca9bc065ed59e3cdd3bc9a9985335  OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi-v0.2.uf2
88c1904d3f18c33f50b9b0481f22465905930bff12d3eccff9ab407ec4710b77  OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi-v0.2.bin
```

## Flashing

BOOT1 + RESET → unitate USB **RPI-RP2** → copiază fișierul `.uf2`.
Nu s-a scris niciun fișier pe hardware în cadrul pregătirii acestui release.

Licență: GPL-3.0 (ca pico-speccy / ESPectrum original).
