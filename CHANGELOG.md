# Changelog

All notable changes to this project will be documented in this file.

## [March 25, 2026]

### Updated
- **WLED Base**: Updated to latest main branch from WLED-dev/WLED (commit d8cb20a9, March 24 2026)
- **Build Date**: March 25, 2026 22:28 UTC
- **Firmware**: firmware.bin (1.2M)
- **Bootloader**: bootloader.bin (16K)
- **Partitions**: partitions.bin (3.0K)

### Notable Fixes in Latest WLED Build
- Bugfix: Prevent _segment_index being misaligned w.r.t. segments vector
- Strip.service bugfix: Avoid losing trigger events
- SHA256 hash string fix
- Stencil blendingmode and speed optimizations
- Prevent _usedSegmentData Counter Underflow During Mode Blending

### Configuration
- Platform: Espressif 32 (2024.6.0)
- WLED Version: 16.0.0-alpha
- Flash: 81.8% (1286229 bytes of 1572864 bytes)
- RAM: 24.9% (81588 bytes of 327680 bytes)
- Custom build for ESP32-32E MOSFET Board
  - 4MB Flash
  - Flash Mode: DOUT @ 40MHz
  - Custom Partition Table

---

## [Previous Builds]

Baseline firmware built from WLED main branch with custom partition configuration for ESP32-32E MOSFET board.
