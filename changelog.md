---
layout: page
title: Changelog
permalink: /changelog.html
---

Dated milestones of the port. Test builds only; nothing has been released.

## 2026-11-21

- **Wi-Fi suspend filter** (kernel): while asleep, the Wi-Fi chip now drops mDNS and SSDP
  multicast instead of waking the tablet. Night standby: about 0.55 %/h drain and 4.3 % awake
  (earlier night: 0.87 %/h and 13.2 %, but with an adb connection open that also woke the tablet).
  See [Technical notes](technical-notes.html#wi-fi-suspend-filter).
- **Launcher and SystemUI crash fixes** (frameworks/base): Recents no longer crashes the
  launcher when the shell is busy; the status bar no longer crashes after its view is destroyed.

## 2026-11-20

- **microSD works.** The tablet's card slot is on a different controller than the Z3
  phones'; z3tc now has its own fstab.
- **Charge limiter switch:** Quick Settings tile "Charge limit" and a settings screen
  under Settings > Battery. See [Technical notes](technical-notes.html#charge-limiter).
- **Mobile network:** single-SIM radio setup; LTE data, calls and SMS tested.
- Camera SELinux policy, VINTF target level 3, Wi-Fi hotspot name, and a status bar
  battery icon fix (correct icon while the limiter pauses charging).
- Settings > Battery crash (LineageParts) analysed; fix in lineage-sdk prepared.

## 2026-11-19

- **Hardware reset fix:** the tablet no longer resets under load. Runs with all CPU
  cores online and applies boosts directly through the power HAL.
  See [Technical notes](technical-notes.html#reset-fix).
- **SurfaceFlinger fix:** app close and dialog animations are smooth; GPU load is about
  a quarter of before. See [Technical notes](technical-notes.html#surfaceflinger-fix).
- Acceptance test passed: 3 × 9 rounds of app cycling, no reset, no crash.

## 2026-11-17

- **Deep Doze** works without a significant-motion sensor.
- First version of the charge limiter (root only).

## 2026-11-16

- Phone-style layout (density 340) and the gesture navigation bar hidden while the
  notification shade is open, so the shade animates on the display hardware instead of the GPU.

## 2026-11-15

- **Camera:** camera HAL v1 support restored in cameraserver, and the Camera app falls
  back to the camera1 API. Photos work with both cameras.
- **Random reboots fixed:** double free in the Broadcom Bluetooth line discipline (kernel).
- **Bluetooth HAL crash loop fixed:** stale read offset in the Bluetooth driver (kernel).
- **Restart hang fixed:** the device is kept awake while the framework restarts.
- **Wi-Fi HAL crash fixed** on every framework restart (wifi_hal command cleanup).
- SystemUI privacy dot disabled (saves display hardware layers).
- All changed projects published as public forks under [github.com/codfx](https://github.com/codfx).
