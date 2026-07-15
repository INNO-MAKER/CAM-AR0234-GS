# CAM-AR0234-GS

**MIPI CSI-2 2.3MP Global Shutter Camera Module (onsemi AR0234 Monochrome)**

---

## 1. Product Overview

The **CAM-AR0234-GS** is a compact MIPI CSI-2 industrial camera module featuring the **onsemi AR0234CS** monochrome global shutter CMOS sensor. With a native resolution of **1920×1200 @ 120 fps** over a 4-lane MIPI CSI-2 interface, it is designed for demanding machine vision, robotics, and embedded AI vision applications.

The module ships with a **YTOT YT10115-5MP+IR0623 6mm F1.0 starlight lens** (no IR-cut filter), making it ideal for near-IR and low-light night vision applications when paired with IR illumination.

### Key Features

- **Starlight ultra-large aperture F1.1 (~F1.0)** for imaging under low light
- **6mm focal length**, M12×P0.5 board mount; 5MP resolving power fully covers the AR0234 frame
- **No IR-cut filter** — full-spectrum, passes near-IR; pairs with IR illumination for night vision
- **AR0234 global shutter** — no jello on fast motion; 4-lane CSI-2, 1920×1200 @ 120 fps measured
- **External hardware trigger + Strobe (flash sync)** output for multi-camera sync / strobed illumination
- **Compact 32×32mm board**; low distortion (TV −5.1%), RoHS, industrial −20 to +60 °C

### Applications

- Security & Surveillance (24/7 monitoring)
- Industrial & Machine Vision
- Embedded / Edge AI Vision
- IR-illuminated night vision

---

## 2. Specifications

### 2.1 Sensor & Imaging

| Parameter | Specification |
|-----------|--------------|
| Sensor Model | AR0234CS (onsemi, Monochrome, Global Shutter) |
| Sensor Type | CMOS Global Shutter (Pregius-class) |
| Effective Resolution | 2.3MP |
| Optical Format | 1/2.6" |
| Pixel Size | 3.0 µm × 3.0 µm |
| Active Pixels | 1920 (H) × 1200 (V) |
| Max Frame Rate | 120.45 fps @ 1920×1200 |
| ADC Bit Depth | 10-bit (RPi R10_CSI2P mode) / sensor supports up to 12-bit |
| Dynamic Range | ~71 dB (typ.) |
| Low Light | Monochrome + no IR-cut; near-IR sensitive; strong low-light / night performance |

### 2.2 Interface

| Parameter | Specification |
|-----------|--------------|
| Interface | MIPI CSI-2 |
| Data Lanes | 4-lane |
| Data Rate | 450 MHz / 900 Mbps per lane (DDR) |
| Compatibility | Raspberry Pi 5 (rp1-cfe / PiSP), NVIDIA Jetson, embedded systems |

### 2.3 Lens — YT10115-5MP+IR0623

| Parameter | Specification |
|-----------|--------------|
| Mount | M12 × P0.5 (board lens) |
| Focal Length | 5.99 mm (~6 mm), ±5% |
| Field of View | H ~56.6° / V ~34° / D ~66–67° |
| Aperture | F1.1 ±10% (starlight ultra-large aperture) |
| IR Filter | **No IR-cut filter** — datasheet §5.3 lists an IR-FILTER spec (cut 700–1050 nm), but the actual product ships **without** IR-cut; full-spectrum, passes near-IR |
| Distortion | Optical −14% / TV −5.1% |

### 2.4 Hardware

| Parameter | Specification |
|-----------|--------------|
| Board Size | 32 × 32 mm |
| Lens Body | Dia. 14 mm, M12 mount, total length 22.28 mm |
| Mounting | M12 × P0.5 lens holder, clamp torque 60–600 gf·cm |
| External Trigger | Hardware trigger (GPIO, opto-isolated) |
| Strobe Output | Flash sync output |
| Operating Temperature | −20 °C to +60 °C |

---

## 3. Video Modes

| Resolution | Advertised Frame Rate | Measured Frame Rate | Format | Notes |
|-----------|----------------------|--------------------|---------|----|
| 960×600 | 236.85 fps | ~258.7 fps @ 2ms shutter | R10_CSI2P | Use `--nopreview --save-pts`; preview cannot run this mode |
| 1280×720 | 198.49 fps | ~219.0 fps @ 2ms shutter | R10_CSI2P | `--framerate` must be ≤198 or mode auto-swaps to 960×600 |
| 1920×1080 | 133.58 fps | — | R10_CSI2P | |
| 1920×1200 | 120.45 fps | — | R10_CSI2P | Native full sensor |

---

## 4. Measured Hardware Parameters (Raspberry Pi 5)

| Parameter | Value |
|-----------|-------|
| Sensor Model | ar0234 |
| I2C Address | bus 10, addr 0x10 |
| Interface | MIPI CSI-2 (4-lane) |
| Link Freq / Data Rate | 450 MHz / 900 Mbps per lane (DDR) |
| Platform | Raspberry Pi 5 |
| ISP | PiSP / Raspberry Pi ISP |
| Device Node | /dev/video0 |
| Media Controller | /dev/media2 |
| Driver | rp1-cfe |

---

## 5. Driver Installation

### 5.1 Raspberry Pi 5 (kernel 6.12.47+rpt-rpi-2712)

```bash
tar -xzf ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712.tar.gz
cd ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712
sudo ./install.sh
sudo reboot
```

> Tested on Raspberry Pi 5, kernel **6.12.47+rpt-rpi-2712**.

### 5.2 NVIDIA Jetson (kernel 5.15.148)

```bash
tar -xzf ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz
cd ar0234_tegra_binary_k5.15.148_working_20260710_v1_0
sudo ./install.sh
sudo reboot
```

> Tested on NVIDIA Jetson, kernel **5.15.148**.

---

## 6. Repository Structure

| File | Description |
|------|-------------|
| [`ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712.tar.gz`](./ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712.tar.gz) | Pre-built binary driver for Raspberry Pi 5 (kernel 6.12.47+rpt-rpi-2712), v1.0.0 |
| [`ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz`](./ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz) | Pre-built binary driver for NVIDIA Jetson (kernel 5.15.148), v1.0, 2026-07-10 |

---

## Support

| | |
|---|---|
| Website | [www.inno-maker.com](https://www.inno-maker.com) |
| GitHub | [www.github.com/inno-maker](https://www.github.com/inno-maker) |
| Sales | sales@inno-maker.com |
| Support | support@inno-maker.com |
