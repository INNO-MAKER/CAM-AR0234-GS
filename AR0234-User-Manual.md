# MIPI Camera Manual

# 1. Product Description

**AR0234 MIPI Camera Module (mono) + YTOT YT10115 6mm F1.0 Starlight Lens - Product Description**

---

## Product Overview

The **ar0234 (AR0234CS, monochrome)** is a 2.3MP CMOS Global Shutter MIPI CSI-2 image sensor (onsemi global-shutter CMOS), native 1920x1200, delivered over a **MIPI CSI-2 4-lane** interface to Raspberry Pi 5 (rp1-cfe / PiSP), embedded systems.

---

## Sensor Specifications

| Parameter | Specification |
|---|---|
| **Sensor Model** | ar0234 (AR0234CS, monochrome) |
| **Sensor Type** | CMOS Global Shutter |
| **Pixel Technology** | onsemi global-shutter CMOS |
| **Effective Resolution** | 2.3MP |
| **Optical Format** | 1/2.6" |
| **Pixel Size** | 3.0 um x 3.0 um |
| **Active Pixels** | 1920 (H) x 1200 (V) |
| **Max Resolution** | 1920x1200 |
| **Frame Rate** | 120.45 fps @ 1920x1200 (measured) |
| **ADC Bit Depth** | 10-bit (RPi R10_CSI2P mode) / sensor supports up to 12-bit |
| **Dynamic Range** | TBD: sensor datasheet (AR0234 typ. ~71 dB) |
| **Low Light** | Monochrome + no IR-cut; near-IR sensitive; strong low-light / night performance |
| **Power** | TBD: sensor datasheet |

---

## Video Format & Resolution/Frame Rate

### Detected Camera Modes (from Linux v4l2/media-ctl/rpicam)

| Resolution | Frame Rate | Format | Notes |
|---|---|---|---|
| **960x600** | **236.85 fps (advertised) / measured ~ 258.7 fps @2ms shutter** | R10_CSI2P | Measured (PTS method): ~258.7 fps, peak 262 fps; exceeds advertised default (minimal blanking). Preview cannot run this mode; measure via --nopreview + --save-pts |
| **1280x720** | **198.49 fps (advertised) / measured ~ 219.0 fps @2ms shutter** | R10_CSI2P | Measured (PTS method): ~219.0 fps, peak 220 fps; exceeds advertised default. Preview cannot run this mode; use --nopreview + --save-pts. Note: --framerate must be <=198 or the mode is auto-swapped to 960x600 |
| **1920x1080** | **133.58 fps** | R10_CSI2P |  |
| **1920x1200** | **120.45 fps** | R10_CSI2P | native full sensor |

### Interface Specifications

| Parameter | Specification |
|---|---|
| **Interface** | MIPI CSI-2 |
| **Data Lanes** | 4-lane |
| **Data Rate** | 450 MHz / 900 Mbps per lane (DDR) |
| **Connector** | TBD: module datasheet (e.g. 15/22-pin FFC) |
| **Compatibility** | Raspberry Pi 5 (rp1-cfe / PiSP), embedded systems |

### Supported Pixel Formats

- R10_CSI2P

---

## Industry Applications

- **Security & Surveillance (24/7 monitoring)**
- **Industrial & Machine Vision**
- **Embedded / Edge AI Vision**
- **IR-illuminated night vision**

---

## Key Features

✅ **Starlight ultra-large aperture F1.1 (~F1.0) for imaging under low light**
✅ **6mm (5.99mm) focal length + M12 x P0.5 board mount; 5MP (2592x1944) resolving power fully covers the AR0234 frame (image circle dia. 7)**
✅ **No IR-cut filter, full-spectrum passes near-IR; pairs with IR illumination for night vision -- a natural match for the AR0234 monochrome sensor**
✅ **AR0234 global shutter, no jello on fast motion; 4-lane CSI-2, 1920x1200@120fps measured**
✅ **External hardware trigger + Strobe (flash sync) output for multi-camera sync / strobed illumination**
✅ **Compact 32x32mm board; low distortion (TV -5.1%), RoHS, industrial -20 to +60 degC**

## Lens Specifications

| Parameter | Specification |
|---|---|
| **Mount** | M12 x P0.5 (board lens) |
| **Focal Length** | 5.99 mm (~6 mm), +/-5% |
| **Field of View** | Mapped to AR0234 (5.76x3.6mm, dia. 6.79); closest to datasheet 1/2.7" row: H ~56.6 deg / V ~34 deg / D ~66-67 deg (H matches 1/2.7"=5.76mm width; V/D slightly larger due to AR0234 full 3.6mm height vs datasheet V31.2/D65.5) |
| **Aperture** | F1.1 +/-10% (starlight ultra-large aperture; cover marked ~F1.0) |
| **IR Filter** | NO IR-cut filter -- datasheet 5.3 lists an IR-FILTER spec (cut 700-1050nm), but the actual product ships WITHOUT IR-cut; full-spectrum, passes near-IR; suited to starlight night vision + AR0234 monochrome |
| **Distortion** | Optical distortion -14% / TV distortion -5.1% (@1/2.7") |

## Hardware Specifications

| Parameter | Specification |
|---|---|
| **Dimensions** | Board 32 x 32 mm; lens body dia. 14, M12 mount, total length 22.28mm (in air) |
| **Board Size** | 32 x 32 mm |
| **Weight** | TBD |
| **Pcb Size** | 32 x 32 mm |
| **Mounting** | M12 x P0.5 lens holder / clamp torque 60-600 gf.cm |
| **Cable** | TBD: module datasheet |
| **External Trigger** | External hardware trigger (GPIO) |
| **Strobe** | Strobe (flash sync) output |


# 2. Measured Hardware Parameters (Linux live detection)

## Camera 1

### Basic Information

| Parameter | Value |
|---|---|
| **Sensor Model** | ar0234 |
| **I2C Address** | bus 10, addr 0x10 |
| **Interface** | MIPI CSI-2 (4-lane) |
| **Link Freq / Data Rate** | 450 MHz / 900 Mbps per lane (DDR) |
| **Platform** | Raspberry Pi |
| **ISP** | PiSP / Raspberry Pi ISP |
| **Device Node** | /dev/video0 |
| **Media Controller** | /dev/media2 |
| **Driver** | rp1-cfe |

### Video Formats & Resolution/Frame Rate _(source: rpicam-hello)_

| Format | Resolution | Frame Rate (fps) |
|---|---|---|
| **R10_CSI2P** (rpicam) | 960x600 | 236.85 |
|  | 1280x720 | 198.49 |
|  | 1920x1080 | 133.58 |
|  | 1920x1200 | 120.45 |
| **Max Resolution** | 1920x1200 | 120.45 |