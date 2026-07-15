# CAM-AR0234-GS

**MIPI CSI-2 2.3MP Global Shutter Camera Module (onsemi AR0234 Monochrome)**

---

## Overview

The **CAM-AR0234-GS** is a compact MIPI CSI-2 industrial camera module featuring the **onsemi AR0234CS** monochrome global shutter CMOS sensor. With a native resolution of **1920×1200 @ 120 fps** over a 4-lane MIPI CSI-2 interface, it is designed for demanding machine vision, robotics, and embedded AI vision applications.

The module ships with a **YTOT YT10115-5MP+IR0623 6mm F1.0 starlight lens** (no IR-cut filter), making it ideal for near-IR and low-light night vision applications when paired with IR illumination.

---

## Key Specifications

| Parameter | Value |
|-----------|-------|
| Sensor | onsemi AR0234CS (Monochrome, Global Shutter) |
| Effective Resolution | 2.3MP (1920 × 1200) |
| Optical Format | 1/2.6" |
| Pixel Size | 3.0 µm × 3.0 µm |
| Max Frame Rate | 120.45 fps @ 1920×1200 |
| Interface | MIPI CSI-2, 4-lane, 900 Mbps/lane |
| Lens | YT10115-5MP+IR0623, 6mm F1.0, No IR-cut filter |
| Board Size | 32 × 32 mm |
| Compatibility | Raspberry Pi 5 (rp1-cfe / PiSP), NVIDIA Jetson (kernel 5.15.148), embedded systems |

---

## Video Modes

| Resolution | Frame Rate | Format |
|-----------|-----------|--------|
| 960×600 | 236.85 fps (up to ~258 fps measured) | R10_CSI2P |
| 1280×720 | 198.49 fps (up to ~219 fps measured) | R10_CSI2P |
| 1920×1080 | 133.58 fps | R10_CSI2P |
| 1920×1200 | 120.45 fps | R10_CSI2P |

---

## Repository Structure

| File / Directory | Description |
|-----------------|-------------|
| [`AR0234-User-Manual.md`](./AR0234-User-Manual.md) | Full user manual: sensor specs, video modes, hardware parameters, and usage guide |
| [`ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz`](./ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz) | Pre-built binary driver for NVIDIA Jetson (kernel 5.15.148), v1.0, 2026-07-10 |

---

## Driver Installation (NVIDIA Jetson — kernel 5.15.148)

```bash
tar -xzf ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz
cd ar0234_tegra_binary_k5.15.148_working_20260710_v1_0
sudo ./install.sh
sudo reboot
```

> Tested on kernel **5.15.148**. Refer to the archive contents for full installation instructions.

---

## Applications

- Security & Surveillance (24/7 monitoring)
- Industrial & Machine Vision
- Embedded / Edge AI Vision
- IR-illuminated night vision

---

## Support

| | |
|---|---|
| Website | [www.inno-maker.com](https://www.inno-maker.com) |
| GitHub | [www.github.com/inno-maker](https://www.github.com/inno-maker) |
| Sales | sales@inno-maker.com |
| Support | support@inno-maker.com |
