# 1.1.3-Bizi (v0.1) — Olimex RP2350PC

Emulator ZX Spectrum (pico-speccy 1.1.3) pentru placa standalone **Olimex RP2350PC** (RP2350B, netlist Rev.A1), HDMI HSTX 640×480@60, flash 4 MB, MinSizeRel, Pico SDK 2.3.1.

**Status: hw-verificat de utilizator** — build `r6`, I2S/ES8311 funcțional pe hardware real.

## Ce este corectat față de commit-ul de bază (736663c)

1. **REG02 = 0x18 (MULT_PRE ×8 → 256 × fs)** — `0x10` (128 × fs) era sub minimul 256 × fs din datasheet ES8311 → codec complet mute. Fix **confirmat pe hardware**: I2S sună.
2. **Volumul din meniu ajunge și la codec** (`pwm_audio_set_volume` → `es8311_set_dac_volume`); boot-ul pornește pe slider-ul salvat.
3. **Fix inversare de scară:** firmware 0=mut…16=tare vs REG32 `255−16v` (16=tare) → argumentul codec = `16−vol`; altfel slider-max = mute.
4. **Cap la capătul tare:** codec-arg ≥ 2 (≈ −16 dB, nivelul istoric acceptabil) — ultimele poziții de slider nu mai depășesc; slider min = mute păstrat.
5. **`pcm_cleanup` nu mai scrie REG32 = 0** — mute-ul nu mai persistă după reset mașină / pauză / F11.

## Config placă (în această imagine)

| Subsistem | Detalii |
|---|---|
| Video | **HDMI HSTX 640×480@60** (lane-uri D0=12/13, CK=14/15, D2=16/17, D1=18/19; doar back-end HSTX). Fără VGA. |
| Audio — difuzoare | PWM GP26/27 (L/R) → PAM8003; I2S/ES8311 însumat analogic. |
| Audio — căști | I2S/ES8311 pe jack (HP-out codec): DATA=GP31, BCK=GP30, LRCK=GP29, fără MCLK extern; codec I2C 0x18 pe GP32/33; GP22 (LDO), GP35 (amp), GP34 (volum analog). |
| PSRAM | 8 MB APS6404L pe QMI CS1 = GPIO8. |
| SD | SPI1: CS=9, SCK=10, MOSI=11, MISO=24. |
| Tastatură | USB (hub intern, `KBDUSB`). |
| Debug UART | TX-only GP0. WiFi: ESP8266 GP38/39. |

## Artefacte + SHA-256

- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.uf2` — `608e585149659006c8ac070ddb0470051991c4b74c1e9fb9738b3b871692d7be`
- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.bin` — `359b8346df9d1bd6fb09ac5d4f7d33d27524d7dfcda4e7e2476f8aa77262dfae`

Surse: tag `v1.1.3-Bizi-v0.1` (commit `21ec3a6`). Sursă verificabilă prin rebuild bit-exact (repetați comanda standard MinSizeRel/RP2350PC din repo; timestamp-urile `__DATE__`/`__TIME__` încorporate și hash-ul git afișat în OSD sunt singurele valori dependente de mediu).

## Flashing

1. Țineți **BOOT1** în timp ce apăsați **RESET** → unitate USB **RPI-RP2**.
2. Copiați `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.uf2` → reboot automat în emulator.
3. Alternativ: `picotool load -f OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi-v0.1.uf2`.

## Limitări cunoscute

- Jack = HP-out direct codec (PWM nu ajunge în căști); jack introdus mută automat PAM8003 (hardware).
- ES8311 = codec mono (OUTP→HP-L, OUTN→HP-R, același semnal anti-fază — proprietate a plăcii).
- HDMI audio: nivel structural −6 dBFS.
- BCLK = exact 1.000 MHz, la limita practică a datasheet-ului pentru MULT_PRE ×8 la DVDD 3.3 V — experimentul 64fs/2 MHz (v0.2) abordează exact asta.

Licență: GPL-3.0 (ca pico-speccy / ESPectrum original).
