---
layout: page
title: Known issues
permalink: /known-issues.html
---

Status of open issues and recent fixes. Items move from **Investigating** to **Fixed, not
yet in a build** when a fix is tested on the test tablet, and to **In the current build**
once a test build contains it. Every fix is also listed in the [Changelog](changelog.html).

## Investigating

- **The SoC never reaches its deepest sleep state.** The power manager's counters show
  VDD minimisation many times a night but XO shutdown (main clock off) zero times;
  something keeps a vote on the main clock.

## Partly working

- **Recovery: no adb sideload in the 19.1 recovery.** adb shows "offline" on the computer.
  The USB adb fix below should cover it once the recovery is rebuilt; not tested yet.
- **SELinux is permissive.** Enforcing mode is planned before release, followed by a
  re-test of the camera and charge limiter policies.
- **exFAT microSD cards are untested.** vfat cards mount, eject and re-mount correctly.

## Occasional

- **"Ghost" of a closing app.** Rarely, a faded image of the app that is closing stays on
  screen for a few seconds when another app opens or the screen unlocks. Not reproduced
  in recordings yet.

## Fixed, not yet in a build

These are tested on the test tablet (installed by hand) and will be in the next build.

- **Charge limit: paused indicator** (device/sony/z3tc, frameworks/base): while the charge
  limit has paused charging, the status bar battery shows a small heart instead of the bolt
  (default and circle styles) for the whole pause, until charging starts again at the lower
  level, and a silent notification "Charging paused at X%" sits in the shade (tap opens the
  settings, swipe to dismiss). The tile and the settings screen now stay
  in sync, and the charge limit has a battery-with-heart icon.
- **USB adb** (kernel, packages/modules/adb): adb over a USB cable works, and Wi-Fi adb no
  longer hangs while a cable is connected. Android 12's adb needs asynchronous USB I/O that
  this 3.4 kernel lacks; marcogiannetta-pi's legacy USB support for adbd and his USB kernel
  fixes solve it. Tested: large adb and MTP transfers both ways, 20 plug/unplug cycles.
- **TrustZone helper restart loop** (device/sony/shinano-common): the `suntrold` service
  restarted about every 23 seconds with the screen off. Its start script waited with
  `tail -f /dev/null`, and on this 3.4 kernel that wait fails each time the system tries to
  suspend, so the service ended and was started again. The script now waits with a sleep
  loop. Tested: the new wait survives the same interruption that ended `tail`, uses no CPU,
  and the camera still works; the next overnight standby test confirms it on battery.

## In the current build

The current test build (2026-11-22) contains:

- **Video recording** (frameworks/av, device/sony/msm8974-common): Sony's camera blob only
  supports camera HAL v1, whose recording path Android 12 had removed. The path is restored,
  and the camera now hands video frames over as plain YUV copies in the encoder's own layout.
  Recording works at 720p and 1080p with sound, 30 fps and correct colours.
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
- SurfaceFlinger fix (smooth app close and dialog animations) and the hardware reset fix.
- Charge limiter with Quick Settings tile and settings screen; correct status bar battery
  icon while the limiter pauses charging.
- Single-SIM mobile radio (LTE data, calls, SMS), camera SELinux policy, VINTF target
  level 3, Wi-Fi hotspot name.

See the [Changelog](changelog.html) for the full list.

## Standby

- Night standby with the Wi-Fi suspend filter: about 0.55 %/h, awake 4.3 % of the time.
  Bluetooth and the battery gauge are now the largest wake sources.
- Build 2026-11-22, 17.6 h standby: awake 4.5 % of the time, deep Doze 94 %, no crashes.
  The percentage fell faster partway through (about 1 % every 14 minutes with no extra
  activity); the battery gauge's charge counter shows this was the gauge correcting its
  reading: the real drain was about 0.8 %/h.
- Re-measure (14.5 h, including daytime): about 0.65 %/h, awake 4.1 % of the time; the charge
  counter and the percentage agree.
- Deepest sleep state not reached: see **Investigating** above.
- A Qualcomm TrustZone helper (`suntrold`) restarted about every 23 seconds with the
  screen off; fixed, see **Fixed, not yet in a build** above. The next standby test
  measures the effect.
