---
layout: page
title: Known issues
permalink: /known-issues.html
---

Status of open issues and recent fixes. Items move from **Investigating** to **Fixed, not
yet in a build** when a fix is tested on the test tablet, and to **In the current build**
once a test build contains it. Every fix is also listed in the [Changelog](changelog.html).

## Investigating

- **USB OTG: the tablet restarted when a USB stick was plugged in.** The stick failed to
  enumerate, then the tablet restarted by itself; after the restart the stick was detected.
  Not reproduced in 11 plug tests (slow, wobbly, half-inserted, rapid, screen off and
  asleep): every full insertion was detected in about 1 s. The restart followed a failed
  detection of the stick, which these tests never caused. USB sticks otherwise work (NTFS
  tested).

- **Brief white flash during app open and close animations.** For about 0.2 s the screen
  turns white while an app opens or closes to the home screen. Screen recordings show it on
  the previous build too, so it is not new; it is more visible with the tablet layout's larger
  close animation. With animations switched off (Developer options) it almost disappears.
  Suspected cause: the graphics compositor on this GPU during transitions; tests are planned.

- **FM radio app crashes on start.** The app's playback notification has no notification
  channel, which Android 12 rejects. The fix in the app is known; the radio itself is untested.
- **Occasional short audio stutter** while an app is open. Android's audio buffers show no
  gaps; one error on the link between the audio DSP and the codec was logged. Under
  investigation.

## Partly working

- **Recovery: no adb sideload in the 19.1 recovery.** adb shows "offline" on the computer.
  The USB adb fix below should cover it once the recovery is rebuilt; not tested yet.
- **SELinux is permissive.** Enforcing mode is planned before release, followed by a
  re-test of the camera and charge limiter policies.
- **exFAT microSD cards are untested.** vfat cards mount, eject and re-mount correctly.

- **Split screen from Overview with animations switched off.** With all animation scales off
  (Developer options), choosing two apps in Overview only partly starts split screen: one
  half stays empty and the other black. The launcher no longer crashes there (fixed, in the
  next build). Workaround: drag an app from the taskbar to the screen edge, or keep
  animations on - then Overview split works normally.

## Occasional

- **"Ghost" of a closing app.** Rarely, a faded image of the app that is closing stays on
  screen for a few seconds when another app opens or the screen unlocks. Not reproduced
  in recordings yet.

## Hardware of the test tablet (not ROM bugs)

- **Headphones: the right side is quieter on the test tablet.** Android sends equal stereo,
  the codec routing and gains are identical for both sides, and the same headphones sound
  balanced on another device. The codec measures about 38 ohms on the left and 57 ohms on the
  right (at first plug-in even an open circuit), which points to a worn or dirty right contact
  in this unit's headphone jack.

## Fixed, not yet in a build

These are tested on the test tablet (installed by hand) and will be in the next build.

None at the moment.

## In the current build

The current test build (2026-11-25) contains:

- **TrustZone helper restart loop** (device/sony/shinano-common): the `suntrold` service
  restarted about every 23 seconds with the screen off. Its start script waited with
  `tail -f /dev/null`, and on this 3.4 kernel that wait fails each time the system tries to
  suspend, so the service ended and was started again. The script now waits with a sleep
  loop. Tested: the new wait survives the same interruption that ended `tail`, uses no CPU,
  and the camera still works. Confirmed on battery: 0 restarts in a 15.7 h standby test.

- **Home screen pause in tablet layout** (frameworks/base): pressing Home from an app paused
  for about 0.2 s, because the taskbar's reported height differs between the home screen and
  apps and Android re-laid out the whole screen on every switch. The window manager now keeps
  the taskbar's height stable. Tested: the pause is gone (longest frame about 50-90 ms instead
  of 200-250 ms) with gesture and 3-button navigation, and apps, split screen, the keyboard,
  rotation and the lock screen lay out as before.

- **Charge limit: raising the limit while charging is paused** (device/sony/z3tc): charging
  now resumes within a few seconds; before, it stayed paused until the battery fell to the
  resume level or the charger was replugged. Tested on the charger (limit 55 % paused it,
  raising it to 70 % resumed charging about 2 s later).

- **Tablet layout** (device/sony/z3tc): screen density 320 instead of 340, so the launcher
  and SystemUI use the tablet layout (12L taskbar, dock at the bottom in landscape, split
  screen), and the lock screen rotates with the device by default.
- **GPS assistance data** (device/sony/msm8974-common): Android 12 now finds the download
  servers for the satellite assistance data (XTRA), for faster fixes.
- **Bluetooth start-up margin**: the controller gets 8 s instead of 2.9 s to start.
- **Tablet configuration** (device/sony/z3tc): the window manager extensions library that
  Settings and apps use for large-screen layouts is now included.

The previous test build (2026-11-24) added:

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
- Build 2026-11-24 with the TrustZone helper fix, 15.7 h standby: about 0.51-0.59 %/h
  (percentage and charge counter agree), awake 4.1 % of the time, deep Doze 93 %, no reboot,
  no crashes; the helper did not restart once (before: about 2,200 times a night).
  Double tap to wake was on all night.
- Checked: the SoC reaches its deepest sleep state (XO shutdown with minimised voltage)
  for 82 % of a night. The earlier "XO shutdown zero times" reading was the shallower
  counter, which the power manager skips when it can go straight to the deepest state.
