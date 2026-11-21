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

- **Settings > Battery crash (LineageParts).** LineageOS' charging control service is
  not present on this device and the settings code threw an exception. The fix (in
  lineage-sdk) is written and tested and will be in the next build.

## Standby

- Standby drain is under investigation. After the Wi-Fi suspend filter, Bluetooth and
  the battery gauge are the largest wake sources. A Qualcomm TrustZone helper
  (`suntrold`) restarts about every 23 seconds; it is not the cause of any crash.
