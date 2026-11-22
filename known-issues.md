---
layout: page
title: Known issues
permalink: /known-issues.html
---

Status of open issues and recent fixes. Items move from **Investigating** to **Fixed, not
yet in a build** when a fix is tested on the test tablet, and to **In the current build**
once a test build contains it. Every fix is also listed in the [Changelog](changelog.html).

## Investigating

- **Video recording does not work.** Photos work with both cameras, but recording fails:
  Sony's camera blob only supports camera HAL v1, and Android 12's video path asks for a
  buffer mode the HAL v1 client does not support. A work-in-progress patch exists; it is
  not in the build.
- **The SoC never reaches its deepest sleep state.** The power manager's counters show
  VDD minimisation many times a night but XO shutdown (main clock off) zero times;
  something keeps a vote on the main clock.

## Partly working

- **Recovery: no adb sideload in the 19.1 recovery.** adb shows "offline" on the computer.
  A related issue: USB adb in Android can hang with a data cable connected (Wi-Fi adb works).
- **SELinux is permissive.** Enforcing mode is planned before release, followed by a
  re-test of the camera and charge limiter policies.
- **exFAT microSD cards are untested.** vfat cards mount, eject and re-mount correctly.

## Occasional

- **"Ghost" of a closing app.** Rarely, a faded image of the app that is closing stays on
  screen for a few seconds when another app opens or the screen unlocks. Not reproduced
  in recordings yet.
- **Bluetooth re-enable may take several attempts** after switching it off and on quickly.

## Fixed, not yet in a build

These are tested on the test tablet (installed by hand) and will be in the next build.

- **Adaptive brightness** (device/sony/z3tc): the light sensor reports in steps of about
  10 lux, and room light reads only about 10 lux. A new brightness curve gives each low
  step its own brightness, so auto-brightness now follows the room (tested in a dark room,
  under a ceiling light, near a window and outdoors).
- **Settings > Battery crash (LineageParts).** LineageOS' charging control service is
  not present on this device and the settings code threw an exception. Fix in lineage-sdk.
- **Launcher and SystemUI crashes** (frameworks/base): Recents no longer crashes the
  launcher when the shell is busy; the status bar no longer crashes after its view is destroyed.
- **microSD** (device/sony/z3tc): the tablet's own fstab with the correct card controller.
- **Wi-Fi suspend filter** (kernel): mDNS and SSDP no longer wake the tablet in standby.

## In the current build

The current test build (2026-11-20) contains, among others:

- SurfaceFlinger fix (smooth app close and dialog animations) and the hardware reset fix.
- Charge limiter with Quick Settings tile and settings screen; correct status bar battery
  icon while the limiter pauses charging.
- Single-SIM mobile radio (LTE data, calls, SMS), camera SELinux policy, VINTF target
  level 3, Wi-Fi hotspot name.

See the [Changelog](changelog.html) for the full list.

## Standby

- Night standby with the Wi-Fi suspend filter: about 0.55 %/h, awake 4.3 % of the time.
  Bluetooth and the battery gauge are now the largest wake sources.
- Deepest sleep state not reached: see **Investigating** above.
- A Qualcomm TrustZone helper (`suntrold`) restarts about every 23 seconds; it is not
  the cause of any crash.
