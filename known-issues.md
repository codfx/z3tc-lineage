---
layout: page
title: Known issues
permalink: /known-issues.html
---

Open issues on the current test build. Fixed items move to the [Changelog](changelog.html).

## Not working

- **Video recording.** Photos work with both cameras, but recording fails: Sony's camera
  blob only supports camera HAL v1, and Android 12's video path asks for a buffer mode
  the HAL v1 client does not support. A work-in-progress patch exists; it is not in the build.

## Partly working

- **Auto-brightness reacts in coarse steps.** The light sensor reports values in steps of
  about 10 lux (0, 0.8, 10.4, 22.2, 51.7 …), so ordinary room light sits on the first one
  or two steps and brightness hardly changes indoors. Next: try the stock sensor
  calibration values, or reshape the low end of the brightness curve.
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

- **Settings > Battery crash (LineageParts).** LineageOS' charging control service is
  not present on this device and the settings code threw an exception. Fix in lineage-sdk.
- **Launcher and SystemUI crashes** (frameworks/base): Recents no longer crashes the
  launcher when the shell is busy; the status bar no longer crashes after its view is destroyed.
- **microSD** (device/sony/z3tc): the tablet's own fstab with the correct card controller.
- **Wi-Fi suspend filter** (kernel): mDNS and SSDP no longer wake the tablet in standby.

## Standby

- Night standby with the Wi-Fi suspend filter: about 0.55 %/h, awake 4.3 % of the time.
  Bluetooth and the battery gauge are now the largest wake sources.
- **Investigating: the SoC never reaches its deepest sleep state.** The power manager's
  counters show VDD minimisation many times a night but XO shutdown (main clock off) zero
  times; something keeps a vote on the main clock. A Qualcomm TrustZone helper
  (`suntrold`) restarts about every 23 seconds; it is not the cause of any crash.
