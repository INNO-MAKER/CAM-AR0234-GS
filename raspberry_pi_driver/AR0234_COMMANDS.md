# AR0234 — Common Commands Cheat Sheet

Raspberry Pi 5, AR0234 global-shutter camera. Assumes Package 1 (driver) + Package 2 (libcamera)
installed.

## Enumerate / verify

```bash
rpicam-hello --list-cameras                 # modes, colour "GRBG" vs mono "MONO"
dmesg | grep -i ar0234                       # chip id, link freq, lanes
media-ctl -d /dev/media1 -p | grep -A2 ar0234   # sensor pad fmt: SGRBG10 (colour) / Y10 (mono)
v4l2-ctl --list-devices
cat /sys/module/ar0234/parameters/trigger_mode   # 0 off / 1 ext-trig / 2 sync-sink
```

## Preview (window vs resolution — they are separate!)

```bash
rpicam-hello -t 0                            # default windowed preview
rpicam-hello -t 0 -f                         # FULLSCREEN preview
rpicam-hello -t 0 -p 0,0,1280,800            # preview window at x,y,w,h
rpicam-hello -t 0 -n                         # no preview (headless)
rpicam-hello -t 0 --qt-preview               # Qt window (heavier)
```

**Capture resolution vs preview window are independent — examples:**

```bash
# Same small preview window, different CAPTURE resolutions (window does NOT change):
rpicam-hello -t 0 -p 100,100,640,480 --mode 960:600:10       # capture 960x600,  window 640x480
rpicam-hello -t 0 -p 100,100,640,480 --mode 1920:1200:10     # capture 1920x1200, window 640x480 (same!)

# Same CAPTURE resolution, different preview WINDOWS (capture does NOT change):
rpicam-hello -t 0 --mode 1920:1200:10                         # capture 1920x1200, default window
rpicam-hello -t 0 --mode 1920:1200:10 -f                      # capture 1920x1200, FULLSCREEN window
rpicam-hello -t 0 --mode 1920:1200:10 -p 0,0,1920,1200        # capture 1920x1200, 1920x1200 window

# Stills: --width/--height = the SAVED image size, unrelated to the preview window:
rpicam-still -o big.jpg  --width 1920 --height 1200 -p 0,0,640,480   # 1920x1200 JPEG, small preview
rpicam-still -o crop.jpg --width 1280 --height 720                    # 1280x720 JPEG

# Want the PREVIEW STREAM itself at a resolution (not the window)? use viewfinder-*:
rpicam-hello -t 0 --viewfinder-mode 1920:1200:10 -f          # preview stream 1920x1200, fullscreen
rpicam-hello -t 0 --viewfinder-width 1280 --viewfinder-height 720
```

Rule of thumb: `--width/--height/--mode` → **capture** (saved/encoded frames).
`-p / -f / -n` → **preview window** (on-screen box). `--viewfinder-*` → **preview stream** resolution.

## Stills

```bash
rpicam-still -o img.jpg                                   # default
rpicam-still -o full.jpg --width 1920 --height 1200       # full resolution
rpicam-still --raw -o frame.dng                           # raw Bayer/mono DNG
rpicam-still -o m.jpg --shutter 5000 --gain 2.0 --awbgains 1,1   # fixed exposure
rpicam-still -t 0 -o snap.jpg --keypress                  # capture on Enter
```

## Video

```bash
rpicam-vid -t 5000 -o clip.h264                          # 5 s H.264
rpicam-vid -t 5000 --width 1920 --height 1200 -o full.h264
rpicam-vid -t 0 --inline -o - | ...                      # continuous stream to stdout
rpicam-vid -t 5000 --codec mjpeg -o clip.mjpeg
```

## High frame rate (bypass preview cap)

```bash
# 1920x1200 @ 120 fps (4-lane, 10-bit)
rpicam-vid -t 5000 --mode 1920:1200:10 --framerate 120 --shutter 4000 --gain 4.0 \
           --codec yuv420 --nopreview -o fast.yuv

# 960x600 binned up to 237 fps
rpicam-vid -t 5000 --mode 960:600:10 --framerate 237 --shutter 2000 --gain 4.0 \
           --codec yuv420 --nopreview -o hs.yuv
# shutter must be <= 1/framerate; add light or raise --gain.
```

## Mono module

```bash
# In config.txt: dtoverlay=ar0234,cam0,4lane,mono   (Y10, auto ar0234_mono.json)
rpicam-hello -t 0                                         # true greyscale, no false colour
rpicam-still --tuning-file /usr/local/share/libcamera/ipa/rpi/pisp/ar0234_mono.json -o m.jpg
```

## External trigger + strobe

```bash
# config.txt: dtoverlay=ar0234,cam0,4lane,external-trigger,flash   (then reboot)
echo 1 | sudo tee /sys/module/ar0234/parameters/trigger_mode       # runtime enable
rpicam-hello -t 0 --shutter 10000 --gain 2.0                       # fixed exposure (AGC off!)
rpicam-vid  -t 5000 --shutter 10000 --gain 2.0 --framerate 30 -o trig.h264
```

## Manual controls

```bash
rpicam-hello -t 0 --shutter 8000                 # exposure (us)
rpicam-hello -t 0 --gain 4.0                      # analogue+digital gain
rpicam-hello -t 0 --awbgains 1.0,1.0              # fixed WB (colour)
rpicam-hello -t 0 --hflip --vflip                 # flip
rpicam-hello -t 0 --roi 0.25,0.25,0.5,0.5         # digital zoom (normalised)
rpicam-hello -t 0 --framerate 60                  # cap frame rate
```

## Raw V4L2 (no libcamera)

```bash
# enable CFE link first (else STREAMON -EINVAL)
media-ctl -d /dev/media1 -l '"csi2":4 -> "rp1-cfe-csi2_ch0":0 [1]'
media-ctl -d /dev/media1 --set-v4l2 '"ar0234 10-0010":0 [fmt:SGRBG10_1X10/1920x1200]'
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1200,pixelformat=pgAA
v4l2-ctl -d /dev/video0 --stream-mmap --stream-count=10 --stream-to=raw.data
# sensor subdev controls:
v4l2-ctl -d /dev/v4l-subdev2 --list-ctrls
```

## Install / driver

```bash
sudo ./install.sh                                 # source pkg: build+install
OVERLAY_OPTS="cam0,4lane,mono" sudo ./driver/install.sh   # binary pkg
modinfo -F filename ar0234                         # which module loads (should be updates/dkms)
sudo modprobe -r ar0234 && sudo modprobe ar0234    # reload (may need reboot on some setups)
```
