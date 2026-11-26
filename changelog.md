---
layout: page
title: Changelog
permalink: /changelog.html
---

Dated milestones of the port. Test builds only; nothing has been released.

## 2026-11-26

- **Close animation flicker fixed:** at the end of the swipe-up close a faded full-size copy of
  the app window could flash for one frame; fixed with a change from Android 13's launcher
  (tested frame by frame, not yet in a build).

## 2026-11-25

- **Test build 2026-11-25** contains the tablet layout (320 dpi, taskbar, lock screen
  rotation), the home screen pause fix, the charge limit resume fix, the TrustZone helper fix,
  GPS assistance data servers, a longer Bluetooth start-up margin and the window manager
  extensions. Passed: image check, boot check and a 9-round app-cycle test (0 crashes);
  overnight standby test pending.
- **Split screen from Overview with animations off:** the launcher crashed when the second
  app was chosen; fixed (tested, not yet in a build). The split itself still only partly
  starts with animations off; use the taskbar drag.
- **Close animation flicker:** frame-by-frame capture shows a one-frame flash of the closing
  app's window at the end of the close animation (investigating). An earlier note about white
  frames in screen recordings was wrong: those were gaps in the recordings.
- **Test build 2026-11-24** contains the charge limit paused indicator (heart battery icon,
  silent notification) and USB adb. Passed: boot check, a 9-round app-cycle test, hand checks
  (USB adb and MTP, charge limit pause, video, microSD, brightness) and a 15.7 h standby test:
  about 0.51-0.59 %/h, awake 4.1 %, deep Doze 93 %, no reboot, no crashes.
- **TrustZone helper fix confirmed on battery:** `suntrold` did not restart once in the
  standby test (before: about 2,200 times a night).
- **Double tap to wake checked:** turned on in Settings, it is armed again at every boot,
  and a night with it on drained no more than nights with it off.
- **Tablet layout, gesture navigation:** no grey bars on the lock screen or behind the
  landscape dock; they appear only with 3-button navigation (its bar background).
- **Home screen pause fixed:** in tablet layout, pressing Home from an app no longer pauses
  for about 0.2 s (window manager fix; tested, not yet in a build).
- **Charge limit:** raising the limit while charging is paused now resumes charging without
  a replug (tested, not yet in a build).
- **USB OTG restart not reproduced:** 11 plug tests with the same stick (slow, wobbly,
  half-inserted, rapid, screen off and asleep) all worked; still under investigation.
- **Deepest sleep state checked:** the SoC spends 82 % of a night in its deepest state (XO
  shutdown with minimised voltage); removed from the known issues.

## 2026-11-24

- **Tablet layout:** screen density 320 instead of 340, so the launcher and SystemUI use the
  tablet layout: 12L taskbar, dock at the bottom in landscape, split screen (tested, not yet in
  a build). Being refined: the landscape dock background and the taskbar's navigation buttons.
- **Bluetooth re-enable checked:** 40 quick off/on cycles (1 s and 3 s apart) all came back on
  in about 1.1 s, with no crashes. The earlier "several attempts" problem was fixed by the
  Bluetooth kernel and HAL fixes; removed from the known issues. The next build also allows the
  controller more start-up time as a safety margin.
- **TrustZone helper restart loop fixed:** `suntrold` no longer restarts every ~23 s with the
  screen off (its start script now waits with a sleep loop; tested, not yet in a build).

## 2026-11-23

- **USB adb works:** adbd uses blocking USB I/O again (marcogiannetta-pi's legacy USB patch
  and USB kernel fixes); Wi-Fi adb no longer hangs with a cable connected (tested, not yet in
  a build).

- **Test build 2026-11-22** contains video recording, adaptive brightness, the Settings >
  Battery crash fix (lineage-sdk), the launcher and SystemUI crash fixes, the microSD fstab
  and the Wi-Fi suspend filter. Passed: boot check, a 9-round app-cycle test, hand checks
  (video, microSD, brightness, charge limit tile) and a 17.6 h standby test (awake 4.5 %,
  deep Doze 94 %, no crashes).
- **Charge limit paused indicator:** a heart in the status bar battery and a silent
  "Charging paused" notification while the limit holds charging off (tested, not yet in a build).

## 2026-11-22

- **Video recording works:** restored the camera HAL v1 recording path and pass video frames
  as YUV in the encoder's layout; 720p and 1080p with sound (tested, not yet in a build).
- **Adaptive brightness:** new brightness curve for the light sensor's 10-lux steps;
  brightness now follows room light (tested, not yet in a build).

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
