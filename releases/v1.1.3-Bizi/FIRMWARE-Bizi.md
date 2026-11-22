# Firmware 1.1.3-Bizi — Olimex RP2350PC (port pico-speccy)

Emulator ZX Spectrum (pico-speccy, port al ESPectrum) pentru placa standalone
**Olimex RP2350PC** (RP2350B, netlist Rev.A1).

- **Baza:** build-rp2350pc-r6, configurație **MinSizeRel**, target `RP2350PC`
  (`-DRP2350PC=ON -DHDMI_HSTX=TMDS`), Pico SDK 2.3.1, amprentă flash 4 MB.
- **Domeniu:** release local (snapshot r6), creat doar din fișiere noi —
  build-urile r1..r6 și `firmware/` actual rămân neatinse.

EN: Local release of the r6 build (MinSizeRel, RP2350PC target, SDK 2.3.1,
4 MB flash footprint). New files only; existing builds and `firmware/` untouched.

## Fișiere / Files

- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.uf2` — imagine flash (USB mass-storage / picotool)
- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.bin` — binar brut pentru unelte de flash

SHA-256 (recalculate for the copied `-Bizi` files; byte-identical to the r6 sources):

```
608e585149659006c8ac070ddb0470051991c4b74c1e9fb9738b3b871692d7be  OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.uf2
359b8346df9d1bd6fb09ac5d4f7d33d27524d7dfcda4e7e2476f8aa77262dfae  OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.bin
```

## Config placă / Board configuration in this image

| Subsistem | Detalii |
|-----------|---------|
| Video | **HDMI HSTX 640×480@60** — serializer HSTX + encoder TMDS hardware, lane-uri neregulate pe blocul GPIO12–19: **D0=12/13, CK=14/15 (perechea de ceas la mijloc), D2=16/17, D1=18/19**; doar back-end-ul HSTX îl poate conduce. Fără VGA. |
| Audio — difuzoare | **PWM GP26/27** (L/R) direct în amplificatorul **PAM8003**; **I2S/ES8311** se însumează analogic în PAM8003. |
| Audio — căști (calea activă) | **I2S/ES8311 pe jack (HP-out codec)**: DATA=GP31, BCK=GP30, LRCK=GP29, **fără MCLK extern** (BCLK-driven, REG01 bit7). Codec pe **I2C 0x18**, GP32/33 (shared DDC FET). Control: **GP22** (LDO codec), **GP35** (amp shutdown, active-low), **GP34** (volum analog). |
| PSRAM | 8 MB APS6404L pe **QMI CS1 = GPIO8** (același bus QMI ca flash-ul 16 MB pe CS0). |
| Card SD | **SPI1** pe slotul microSD — CS=9, SCK=10, MOSI(CMD)=11, MISO(DAT0)=24. |
| Tastatură | USB (hub-ul intern al plăcii, 4× USB-A, `KBDUSB`). |
| Debug UART | TX-only pe **GP0** (UART0 / UEXT1). |
| WiFi | **ESP8266 pe GP38/39**. |

## CHANGELOG I2S/ES8311 (acest release / this release)

1. **r3 — REG02 fix `0x10` → `0x18`** (MULT_PRE ×8 → ceas intern 256×fs;
   `0x10` = 128×fs, sub minimul 256×fs din datasheet ES8311 = mute total).
   Confirmat pe hardware: I2S sună.
   EN: REG02 0x10 was 128fs — below the datasheet's 256fs minimum → total
   silence; 0x18 (×8 → 256fs) makes I2S audible (hw-confirmed).
2. **r3 — `pcm_cleanup` nu mai scrie REG32=0** (mute-ul persista după
   reset mașină / F11).
   EN: pcm_cleanup no longer writes REG32=0, so the mute didn't survive a
   machine reset / F11.
3. **r4 — volumul din meniu scrie și codec-ul** (`pwm_audio_set_volume` →
   `es8311_set_dac_volume`); boot-ul folosește slider-ul salvat.
   EN: menu volume now also drives the codec register; boot restores the
   saved slider.
4. **r5 — fix inversare de scală:** firmware 0=mut…16=tare vs REG32
   `255−16v` (16=tare) → argument = `16−vol`. (r4 avea slider-max = mute —
   regresie hw-confirmată.)
   EN: firmware scale (0=mute..16=loud) is inverted vs REG32; the argument
   must always be 16−vol, otherwise slider-max = mute.
5. **r6 — cap la maximul tare:** codec-arg ≥ 2 (≈ −16 dB, nivelul istoric
   acceptabil) — ultimele poziții de slider nu mai depășesc; slider min =
   mute păstrat.
   EN: loud end capped at codec-arg 2 (≈ −16 dB); top slider positions no
   longer exceed it; slider min = mute kept.

## Note / limitări cunoscute (audit hardware Rev.A1)

- Jack-ul = **HP-out direct codec** (PWM NU poate suna în căști).
  EN: the headphone jack is fed by the codec's HP-out only; the PWM path
  cannot reach the jack.
- Căștile introduse în jack mută automat PAM8003 (detect jack, hardware).
  EN: plugging headphones auto-mutes the PAM8003 speakers (hardware jack detect).
- ES8311 = codec **MONO** (OUTP→HP-L, OUTN→HP-R = același semnal anti-fază —
  comportament al plăcii).
  EN: the ES8311 is a mono codec; both jack channels carry the same
  anti-phase signal — a board property.
- PWM + I2S însumate analogic în PAM8003 (difuzoare).
  EN: PWM and I2S outputs are summed analogically into the PAM8003 (speakers).
- HDMI audio: nivel structural −6 dBFS.
- **Întrebare deschisă:** BCLK = exact 1.000 MHz — la limita practică a
  datasheet-ului („>1 MHz" pentru MULT_PRE×8 la DVDD 3.3 V). Dacă se constată
  degradare de calitate, varianta 64fs / 2 MHz este planificată.
  EN: open question — BCLK is exactly 1.000 MHz, at the datasheet's practical
  limit for MULT_PRE×8 at DVDD 3.3 V; a 64fs / 2 MHz variant is planned if
  quality degradation is observed.

## Flashing (USB mass-storage)

1. Ține **BOOT1** (butonul user) în timp ce alimentezi placa / apeși RESET.
2. Apare o unitate USB numită **RPI-RP2**.
3. Copiază `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.uf2` pe ea —
   placa rebootează automat în emulator.

Alternativ cu `picotool`:

```bash
picotool load -f OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3-Bizi.uf2
```

Notă: copierea pe hardware este actul utilizatorului; acest release doar
livrează artefactele.
EN note: copying to physical hardware is the user's action; this release
only ships the artifacts.

## Licență / License

GPL-3.0 (ca pico-speccy / ESPectrum original). Vezi `LICENSE` în rădăcina
repository-ului.
