---
layout: page
title: Install
permalink: /install.html
---

> **Not released yet.** There is no download and no supported install path. This page
> shows how installation is planned to work, so you know what to expect.

## Requirements (planned)

- Sony Xperia Z3 Tablet Compact, model **SGP621** (`z3tc`). Other Z3 models are not supported.
- **Unlocked bootloader.** Unlocking erases the DRM keys (TA partition). Back up TA first
  if you may ever want to return to stock.
- `adb` and `fastboot` (Android platform-tools) on a computer.
- A full backup of your data. Moving from another ROM needs a data wipe.

## Steps (planned, may change)

1. Boot into fastboot mode and flash the recovery image to the FOTAKernel partition.
2. Boot into recovery, then **Factory reset → Format data**.
3. **Apply update → Apply from ADB** and sideload the ROM zip.
4. Optional: sideload add-ons (for example Magisk) before the first boot.
5. Reboot. The first boot takes several minutes.

The 19.1 recovery can sideload, but its screen does not refresh yet, so the release will
either fix it or recommend a known-good recovery. See [Known issues](known-issues.html).

Downloads, checksums and exact file names will be added here at release.
