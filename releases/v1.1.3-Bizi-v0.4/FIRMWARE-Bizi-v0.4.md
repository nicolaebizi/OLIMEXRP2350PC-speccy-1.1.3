# Firmware 1.1.3-Bizi (v0.4) — hw-confirmed — Olimex RP2350PC, varianta 16 MB

- **Baza:** build-rp2350pc-r9c, configurație **MinSizeRel**, target `RP2350PC`
  (`-DRP2350PC=ON -DHDMI_HSTX=TMDS`), Pico SDK 2.3.1, amprentă flash **16 MB**.
- **✅ CONFIRMAT integral pe hardware de utilizator (2026-11-22, „ESTE F. OK").**
- **Flash 16 MB folosit integral:** imagine 7,67 MB (7.674.632 B / 15.350.272 B uf2).
- **Starul versiunii: AUDIO** — calibrare de nivel pe fiecare ieșire + fix distorsiune
  pe calea player MIDI (detaliat mai jos).

## Configurație

- Video: **HDMI HSTX 640×480@60**.
- **MoonSound complet EMBEDDED:** **YRW801 ROM (2 MB)** + **GM bank Microsoft
  (gm.dls, 1.668.026 B, pachetat @31250 Hz, µ-law v5)** — funcționează **fără
  fișiere pe cardul SD**.
- ROM-uri: Hobbit.
- PSRAM 8 MB (QMI CS1, GP8).
- SD pe SPI1 (GP9/10/11/24).
- USB: tastatură + hub.
- Debug UART TX pe GP0.
- WiFi: ESP8266 pe GP38/39.

## AUDIO — calibrare per ieșire (nou în v0.4)

### a) I2S/ES8311 pe jack (calea de căști)

- Varianta **64fs standard-frame (BCLK 2 MHz, REG02 = 0x10)** — toate fix-urile
  I2S incluse și hw-confirmate în r5–r7, r9b.
- Volumul din meniu ajunge **direct în codec**: slider **−32…−96 dB**, cu
  **mute automat la minim**.
- **Cap tare de nivel la −32 dB** (cod codec-arg 4) — plafonul caștilor.

### b) PWM GP26/27 (difuzoare PAM8003)

- **Neschimbat** — rămâne referința sonoră a utilizatorului.

### c) HDMI (ieșire video cu audio integrat)

- **Gain ×3 (+9,5 dB)** pentru corecția „HDMI cronic încet".
- Cu **soft-knee limiter**: knee la 3/4 din full-scale, compresie **8:1** pe
  vârfuri — ×3 pur ar fi clipeat tranzițiile forte.

### d) Calea player MIDI

- **Soft-saturație** (lanțul emulatorului, **plafon −6 dBFS**) în locul
  hard-clipului ×1.5 din MidiDecoder, care distorsiona acordurile de ≥3 voci.
- **s_vol implicit 16 (0 dB)**.

### e) Diagnostic

- Contor **`i2s_drop`** în linia `[PERF] aud:` — activ doar la build cu
  `PERF_TRACE=1`.

## CHANGELOG scurt (v0.1 → v0.4)

| Versiune | Conținut |
|---|---|
| v0.1 | REG02 fix + volum direct în codec + cap tare |
| v0.2 | I2S 64fs standard-frame (BCLK 2 MHz); varianta 4 MB hw-confirmată |
| v0.3 | Flash 16 MB + GM bank embedded (MoonSound fără SD) |
| v0.4 | **MIDI soft-saturație + calibrare de nivel per ieșire** (I2S cap −32 dB, HDMI ×3 soft-knee) |

## Fișiere / Files

- `OLPC-speccy-VGA-HDMI-HSTX-16MB-1.1.3-Bizi-v0.4.uf2` — imagine flash
- `OLPC-speccy-VGA-HDMI-HSTX-16MB-1.1.3-Bizi-v0.4.bin` — binar brut

SHA-256:

```
4c0d23221a4b62e07a219ed1425c89a50d5abaffeb53d922ea2c8e7eb81d4e11  OLPC-speccy-VGA-HDMI-HSTX-16MB-1.1.3-Bizi-v0.4.uf2
98765e32660968fb057f0ba4a00948db12c2d0cf7e9c43bd4f3b9b6239edb515  OLPC-speccy-VGA-HDMI-HSTX-16MB-1.1.3-Bizi-v0.4.bin
```

## Flashing

BOOT1 + RESET → unitate USB **RPI-RP2** → copiază fișierul `.uf2`
(sau `picotool load ... .uf2`).
Nu s-a scris niciun fișier pe hardware în cadrul pregătirii acestui release.

## Note

- **Rebuild-from-source:** cere fișiere locale **nedistribuibile**
  (`yrw801.rom`, `gm.dls` — protejate de licență); nu sunt incluse în repo.
- **Varianta 4 MB rămâne v0.2** (MoonSound din SD): vezi release
  `v1.1.3-Bizi-v0.2`.

Licență: GPL-3.0 (ca pico-speccy / ESPectrum original).
