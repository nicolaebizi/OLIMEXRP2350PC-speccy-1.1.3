# Firmware 1.1.3 — Olimex RP2350PC (port pico-speccy)

ZX Spectrum emulator (pico-speccy, port of ESPectrum) for the **Olimex RP2350PC**
standalone board (RP2350B, Rev.A1 netlist). Built with the `RP2350PC` CMake target
(`-DRP2350PC=ON -DHDMI_HSTX=TMDS`, MinSizeRel, 4 MB flash footprint).

## Board configuration in this image

| Subsystem    | Details |
|--------------|---------|
| Video        | HDMI-only via the RP2350 **HSTX serializer + hardware TMDS encoder**, 640×480@60. Irregular lane order on the GPIO12–19 block: **D0=12/13, CK=14/15 (clock pair in the middle), D2=16/17, D1=18/19** — only the HSTX back-end can drive it; no VGA. |
| Audio        | **PWM** on the PWM5 slice — **GP27 (R) / GP26 (L)** (netlist `PWM_AUDIO-R/L`). I2S/ES8311 codec zone **bypassed** (not driven). |
| PSRAM        | 8 MB APS6404L on **QMI CS1 = GPIO8** (same QMI bus as the 16 MB flash on CS0). |
| SD card      | **SPI1** on the microSD slot — CS=9, SCK=10, MOSI(CMD)=11, MISO(DAT0)=24. |
| Keyboard     | USB (the board's internal hub, 4× USB-A, `KBDUSB`). |
| Debug UART   | TX-only on GP0 (UART0 / UEXT1). |
| MoonSound    | 4 MB image: the YRW801 ROM and GM.DLS bank are **read from the SD card at runtime** (not embedded in flash). |
| Not wired    | No VGA, no NES pad (GPIO5/9/20/21 are UEXT/SD-CS/CEC/HPD), no 595/AY shift-register chain. |
| WI-FI.       | WITH ESP8266 ON GPIO 38 AND 39                |
## Files

- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3.uf2` — flashable image (USB mass-storage / picotool)
- `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3.bin` — raw binary for flasher tools

SHA-256:

```
a320b00ef99a7a48289ab481d97eac6851434063c35b4069d4d7a78f61af7ba3  OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3.uf2
0d54991da9995fda60c1734e5a2396e33a79af999bbd3081085ba29eb31e8a9b  OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3.bin
```

## Flashing (USB mass-storage)

1. Hold **BOOT1** (user button) while powering the board on / pressing RESET.
2. A USB drive named **RPI-RP2** appears.
3. Copy `OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3.uf2` onto it — the board reboots into the emulator automatically.

Alternative with `picotool`:

```bash
picotool load -f OLPC-speccy-VGA-HDMI-HSTX-4MB-1.1.3.uf2
```

Note: a UF2/serial copy to physical hardware is a user action; this repository only ships the artifact.

## License

GPL-3.0 (as the original pico-speccy / ESPectrum). See `LICENSE` in the repository root.
