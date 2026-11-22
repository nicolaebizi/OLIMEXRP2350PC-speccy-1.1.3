# Firmware 1.1.3-Bizi (v0.3) — Olimex RP2350PC — varianta 16 MB

✅ **CONFIRMAT pe hardware de utilizator (2026-11-22).**

## Configurație

- Video: **HDMI HSTX 640×480@60**.
- **Flash 16 MB folosit integral** — imagine 7,67 MB.
- **MoonSound complet EMBEDDED:** **YRW801 ROM (2 MB)** + **GM bank Microsoft
  (gm.dls, 1.668.026 B, pachetat @31250 Hz, µ-law v5)** — funcționează **fără
  fișiere pe cardul SD**.
- ROM-uri: Hobbit. PSRAM 8 MB (QMI CS1, GP8).
- SD SPI1 (GP9/10/11/24); USB tastatură + hub; Debug UART TX GP0; WiFi ESP8266 GP38/39.
- Audio: **PWM GP26/27** (difuzoare PAM8003) + **I2S/ES8311 pe jack** (calea de
  căști), varianta **64fs standard-frame (BCLK 2 MHz, REG02 = 0x10)** — toate
  fix-urile I2S incluse și hw-confirmate (A/B r5–r7).

## Assets & SHA-256

| Fișier | Mărime | SHA-256 |
|---|---|---|
| `OLPC-speccy-VGA-HDMI-HSTX-16MB-1.1.3-Bizi-v0.3.uf2` | 15.349.760 B | `485743b4ad06cd14d96bfe210ef087804c49e01ee85e27fe7a6f89d81c71ee2a` |
| `OLPC-speccy-VGA-HDMI-HSTX-16MB-1.1.3-Bizi-v0.3.bin` | 7.674.616 B | `8212bafe269fc94f19f0b8060217a0e632d75fac3b124e27bafb868dacefa8b6` |

## Flashing

BOOT1 + RESET → unitate USB **RPI-RP2** → copiază fișierul `.uf2`
(sau `picotool load ... .uf2`).

## Note

- **Varianta 4 MB rămâne v0.2** (MoonSound din SD): release `v1.1.3-Bizi-v0.2`.
- **Rebuild-from-source:** cere fișiere locale **nedistribuibile**
  (`yrw801.rom`, `gm.dls` — protejate de licență).

Licență: GPL-3.0 (ca pico-speccy / ESPectrum original).
