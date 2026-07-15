# CAM-AR0234-GS

**MIPI CSI-2 2.3MP Global Shutter Camera Module (onsemi AR0234)**

This repository covers two product variants:

| Model | Sensor | Lens | IR Filter |
|-------|--------|------|-----------|
| **CAM-AR0234-GS-M** | AR0234CS — Monochrome | YT10115-5MP+IR0623, 6mm F1.0 | **No IR-cut filter** — full-spectrum, near-IR sensitive |
| **CAM-AR0234-GS-C** | AR0234CS — Color | YT10115-5MP+IR0623, 6mm F1.0 | **With IR-cut filter** — standard visible-light imaging |

---

## 1. Product Overview

The **CAM-AR0234-GS** series are compact MIPI CSI-2 industrial camera modules featuring the **onsemi AR0234CS** global shutter CMOS sensor. With a native resolution of **1920×1200 @ 120 fps** over a 4-lane MIPI CSI-2 interface, they are designed for demanding machine vision, robotics, and embedded AI vision applications.

Both variants use the **YTOT YT10115-5MP+IR0623 6mm F1.0 starlight lens**, differing only in the lens mount IR filter configuration:

- **CAM-AR0234-GS-M (Monochrome):** Lens mount ships **without** IR-cut filter. Full-spectrum response including near-IR — ideal for night vision, IR-illuminated inspection, and scientific imaging.
- **CAM-AR0234-GS-C (Color):** Lens mount ships **with** IR-cut filter. Accurate color reproduction under visible light — ideal for standard machine vision, quality inspection, and color-sensitive applications.

### Key Features

- **Starlight ultra-large aperture F1.1 (~F1.0)** for imaging under low light
- **6mm focal length**, M12×P0.5 board mount; 5MP resolving power fully covers the AR0234 frame
- **AR0234 global shutter** — no jello on fast motion; 4-lane CSI-2, 1920×1200 @ 120 fps measured
- **External hardware trigger + Strobe (flash sync)** output for multi-camera sync / strobed illumination
- **Compact 32×32mm board**; low distortion (TV −5.1%), RoHS, industrial −20 to +60 °C

### Applications

- Security & Surveillance (24/7 monitoring)
- Industrial & Machine Vision
- Embedded / Edge AI Vision
- IR-illuminated night vision *(CAM-AR0234-GS-M)*
- Color inspection & quality control *(CAM-AR0234-GS-C)*

---

## 2. Specifications

### 2.1 Sensor & Imaging

| Parameter | Specification |
|-----------|--------------|
| Sensor Model | AR0234CS (onsemi, Global Shutter) |
| Sensor Type | CMOS Global Shutter |
| Effective Resolution | 2.3MP |
| Optical Format | 1/2.6" |
| Pixel Size | 3.0 µm × 3.0 µm |
| Active Pixels | 1920 (H) × 1200 (V) |
| Max Frame Rate | 120.45 fps @ 1920×1200 |
| ADC Bit Depth | 10-bit (RPi R10_CSI2P mode) / sensor supports up to 12-bit |
| Dynamic Range | ~71 dB (typ.) |
| Sensor Variant | Monochrome (CAM-AR0234-GS-M) / Color (CAM-AR0234-GS-C) |

### 2.2 Interface

| Parameter | Specification |
|-----------|--------------|
| Interface | MIPI CSI-2 |
| Data Lanes | 4-lane |
| Data Rate | 450 MHz / 900 Mbps per lane (DDR) |
| Compatibility | Raspberry Pi 5 (rp1-cfe / PiSP), NVIDIA Jetson, embedded systems |

### 2.3 Lens — YT10115-5MP+IR0623

| Parameter | CAM-AR0234-GS-M | CAM-AR0234-GS-C |
|-----------|----------------|----------------|
| Mount | M12 × P0.5 | M12 × P0.5 |
| Focal Length | 5.99 mm (~6 mm), ±5% | 5.99 mm (~6 mm), ±5% |
| Field of View | H ~56.6° / V ~34° / D ~66–67° | H ~56.6° / V ~34° / D ~66–67° |
| Aperture | F1.1 ±10% | F1.1 ±10% |
| IR Filter | **No IR-cut filter** — full-spectrum, near-IR sensitive | **With IR-cut filter** — cuts 700–1050 nm, visible light only |
| Distortion | Optical −14% / TV −5.1% | Optical −14% / TV −5.1% |

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
tar -xzf raspberry_pi_driver/precompiler-driver/ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712.tar.gz
cd ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712
sudo ./install.sh
sudo reboot
```

> Tested on Raspberry Pi 5, kernel **6.12.47+rpt-rpi-2712**.

### 5.2 NVIDIA Jetson (kernel 5.15.148)

```bash
tar -xzf jetson-orin-nano-driver/5.15.148/ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz
cd ar0234_tegra_binary_k5.15.148_working_20260710_v1_0
sudo ./install.sh
sudo reboot
```

> Tested on NVIDIA Jetson, kernel **5.15.148**.

---

## 6. Repository Structure

```
CAM-AR0234-GS/
├── README.md
├── raspberry_pi_driver/
│   ├── AR0234_COMMANDS.md                                      ← Common commands cheat sheet
│   ├── ar0234-external-trigger-strobe-v1.0.0.tar.gz           ← External trigger & strobe package
│   ├── pkg2-ar0234-libcamera-source.tar.gz                    ← libcamera source with AR0234 IPA
│   ├── ar0234-rpi-driver-upstream-kurokesu.tar.gz             ← Upstream open-source driver (Kurokesu)
│   └── precompiler-driver/
│       └── ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712.tar.gz
└── jetson-orin-nano-driver/
    └── 5.15.148/
        └── ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz
```

| File | Description |
|------|-------------|
| [`raspberry_pi_driver/AR0234_COMMANDS.md`](./raspberry_pi_driver/AR0234_COMMANDS.md) | Common rpicam / v4l2 commands cheat sheet for AR0234 |
| [`raspberry_pi_driver/ar0234-external-trigger-strobe-v1.0.0.tar.gz`](./raspberry_pi_driver/ar0234-external-trigger-strobe-v1.0.0.tar.gz) | External hardware trigger and strobe (flash sync) support package, v1.0.0 |
| [`raspberry_pi_driver/pkg2-ar0234-libcamera-source.tar.gz`](./raspberry_pi_driver/pkg2-ar0234-libcamera-source.tar.gz) | libcamera source with AR0234 IPA support |
| [`raspberry_pi_driver/ar0234-rpi-driver-upstream-kurokesu.tar.gz`](./raspberry_pi_driver/ar0234-rpi-driver-upstream-kurokesu.tar.gz) | Upstream open-source AR0234 RPi kernel driver by [Kurokesu](https://github.com/Kurokesu/ar0234-rpi-driver) (DKMS, V4L2) |
| [`raspberry_pi_driver/precompiler-driver/ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712.tar.gz`](./raspberry_pi_driver/precompiler-driver/ar0234-binary-v1.0.0-pi5-k6.12.47+rpt-rpi-2712.tar.gz) | Pre-built binary driver for Raspberry Pi 5 (kernel 6.12.47+rpt-rpi-2712), v1.0.0 |
| [`jetson-orin-nano-driver/5.15.148/ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz`](./jetson-orin-nano-driver/5.15.148/ar0234_tegra_binary_k5.15.148_working_20260710_v1_0.tar.gz) | Pre-built binary driver for NVIDIA Jetson (kernel 5.15.148), v1.0, 2026-07-10 |

---

## Support

| | |
|---|---|
| Website | [www.inno-maker.com](https://www.inno-maker.com) |
| GitHub | [www.github.com/inno-maker](https://www.github.com/inno-maker) |
| Sales | sales@inno-maker.com |
| Support | support@inno-maker.com |
