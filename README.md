# ESP32-32E MOSFET Board -- WLED Build

This repository contains prebuilt firmware binaries for the ESP32-32E
4MB Flash MOSFET board.

[Amazon ASIN: B078W5L8W1](https://a.co/d/06iSxmx3)

Board characteristics:

-   ESP32-32E\
-   4MB Flash\
-   Dual OTA layout\
-   SPIFFS filesystem\
-   Flash mode: **DOUT**\
-   Flash frequency: **40MHz**

Firmware layout:

  Address   File
  --------- ----------------
  0x1000    bootloader.bin
  0x8000    partitions.bin
  0x10000   firmware.bin

------------------------------------------------------------------------

# Requirements

-   macOS / Linux / Windows\
-   Python 3\
-   `esptool` installed:

``` bash
pip3 install esptool
```

-   USB-TTL adapter connected to:
    -   TX → RX\
    -   RX → TX\
    -   GND → GND\
-   Board powered from 12V supply (recommended)\
-   Shared ground between adapter and board

------------------------------------------------------------------------

# OTA UPDATE (Recommended - No USB Adapter Needed!)

Once your device is initially flashed, you can update firmware **over-the-air** through the WLED web interface. This is much easier than using the USB-TTL adapter!

## What You Need for OTA
- **Only `firmware.bin`** (the other files are already on your device)
- Device connected to your WiFi network
- Access to WLED web interface

## How to OTA Update

1. **Find your device IP:**
   - Check your router's connected devices
   - Or connect to WLED's AP mode (if WiFi setup fails):
     - Network: `WLED-AP`
     - Password: `wled1234`
     - Device IP: `192.168.4.1`

2. **Open WLED web interface:**
   - Browser: `http://<device-ip>`

3. **Navigate to Update:**
   - Go to **Settings** → **Update**

4. **Upload firmware:**
   - Click "Choose File" and select `firmware.bin`
   - Click "Update" and wait (~30-60 seconds)
   - Device will automatically reboot with new firmware

## Why OTA Works
- **Dual OTA partitions** - Safe updates with automatic fallback
- **No USB adapter needed** - Updates over WiFi
- **Preserves settings** - Configuration stays intact
- **Fast and reliable** - Built into WLED core functionality

## When You Still Need USB-TTL
- **Initial flash** (first time setup)
- **Bootloader/partition changes** (rare)
- **Recovery** (if OTA fails and device won't boot)

------------------------------------------------------------------------

# IMPORTANT: Finding Your Serial Port

The board does not have a USB-To-Serial or similar UART chip so a USB-TTL adapter in required:

[Amazon ASIN: B0G1MM9HNG](https://a.co/d/03ojZ7y5)

In all commands below, `/dev/cu.usbserial-0001` is an example.

You MUST replace it with the correct port name for your system.

## macOS

Unplug adapter, then run:

``` bash
ls /dev/cu.*
```

Plug adapter back in and run again:

``` bash
ls /dev/cu.*
```

The new entry is your port (example: `/dev/cu.usbserial-110`).

## Linux

``` bash
ls /dev/ttyUSB*
ls /dev/ttyACM*
```

Usually something like:

    /dev/ttyUSB0

## Windows

Open Device Manager → Ports (COM & LPT)

Look for:

    USB Serial Device (COM3)

Use:

    --port COM3

------------------------------------------------------------------------

# Enter Flash Mode

1.  Short IO0 → GND\
2.  Apply power\
3.  Remove IO0 short after connection

------------------------------------------------------------------------

# FLASH

Replace the port name before running.

``` bash
esptool --chip esp32 --port /dev/cu.usbserial-0001 --baud 460800 write-flash -z   --flash-mode dout --flash-freq 40m --flash-size 4MB   0x1000  bootloader.bin   0x8000  partitions.bin   0x10000 firmware.bin
```

After flashing completes:

-   Remove IO0 short (if used)
-   Power cycle the board

------------------------------------------------------------------------

# WIPE (Full Chip Erase)

``` bash
esptool --chip esp32 --port /dev/cu.usbserial-0001 erase-flash
```

------------------------------------------------------------------------

# Reset WiFi Settings Only (Clear NVS)

``` bash
esptool --chip esp32 --port /dev/cu.usbserial-0001 erase-region 0x9000 0x5000
```

This restores WLED AP mode.

------------------------------------------------------------------------

# Serial Monitor

``` bash
screen /dev/cu.usbserial-0001 115200
```

Exit screen:

    Ctrl + A, then K, then Y

------------------------------------------------------------------------

# Things You May Need To Change

-   Replace `/dev/cu.usbserial-0001` with your actual port.
-   If flashing fails, lower baud rate to `115200`.
-   If WiFi fails to connect:
    -   Use 2.4GHz only
    -   Disable WPA3-only
    -   Try a simple SSID (no spaces/special characters)
-   If board resets during WiFi:
    -   Use stable 12V supply
    -   Verify solid ground connection

------------------------------------------------------------------------

# Troubleshooting

### Boot loop / invalid header

Ensure firmware is flashed at `0x10000` (not `0x0`).

### Filesystem errors

Ensure partitions.bin matches firmware build.

### No AP after wipe

Verify: - Correct flash mode (`dout`) - Stable 12V supply - Shared
ground with USB-TTL adapter

------------------------------------------------------------------------

# Build Info

**Latest Build: March 25, 2026**

Firmware compiled using PlatformIO\
Based on WLED main branch (commit d8cb20a9, March 24 2026)\
Version: WLED 16.0.0-alpha\
Tested with USB-TTL adapter (example: Amazon ASIN B078W5L8W1)

-   Custom partition table (dual OTA, SPIFFS)
-   Flash mode: DOUT
-   Flash freq: 40MHz
-   Flash size: 4MB
-   Memory: Flash 81.8% / RAM 24.9%

For detailed changelog, see [CHANGELOG.md](CHANGELOG.md)

------------------------------------------------------------------------

GPIO PINS

16 --> CHANNEL1
17 --> CHANNEL2
26 --> CHANNEL3
27 --> CHANNEL4

