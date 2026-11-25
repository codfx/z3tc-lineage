---
layout: home
title: Home
---

Unofficial **LineageOS 19.1 (Android 12)** for the **Sony Xperia Z3 Tablet Compact**
(`z3tc`, SGP621, Snapdragon 801 / msm8974, "shinano").

> **Status: in testing, not released.** There is no download yet. This site documents
> the port, its fixes and what still needs work.

Built on marcogiannetta-pi's shinano LineageOS 19.1 trees for the Z3 Compact, with
z3tc-specific device, kernel and framework changes. All source is public, see
[Sources & credits](sources.html).

## Status

| Area | Status | Notes |
|---|---|---|
| Boot, UI, stability | ✅ Works | No crashes or resets in repeated 9-round app-cycle tests |
| Display / smoothness | ✅ Works | SurfaceFlinger fix: app close animations no longer freeze; tablet layout with taskbar and split screen; brief white flash during app transitions (investigating) |
| Wi-Fi | ✅ Works | Suspend filter for mDNS/SSDP improves standby |
| Bluetooth | ✅ Works | Two kernel crash fixes |
| USB (adb, MTP) | ✅ Works | adb over USB; MTP file transfer |
| USB OTG | ⚠️ Issue | USB sticks work (NTFS tested); one restart on plug-in, not reproduced in 11 tests. Investigating |
| Mobile (LTE, calls, SMS) | ✅ Works | Earpiece and loudspeaker |
| Camera (photos) | ✅ Works | Legacy camera HALv1 support restored; front camera photo and video work |
| Camera (video recording) | ✅ Works | 720p and 1080p with sound |
| GPS | ✅ Works | Slow cold fix without assisted data |
| microSD | ✅ Works | vfat tested; exFAT not yet tested |
| Sensors | ✅ Works | Auto-brightness follows the room (new curve); auto-rotate works |
| Double tap to wake | ✅ Works | Stays on after a restart; no measurable standby cost |
| Headphones, FM radio | ⚠️ Issues | Right headphone channel quiet; FM app crashes on start; occasional audio stutter (investigating) |
| Vibration, screen recording | ✅ Works | Screen recording slows apps slightly while it runs |
| Torch | — N/A | This tablet has no camera flash; the Flashlight tile is hidden automatically |
| Charge limiter | ✅ Works | Quick Settings tile + Settings > Battery; heart battery icon and silent notification while paused |
| Standby / Doze | ✅ Works | About 0.5-0.6 %/h overnight with Wi-Fi on; deep Doze; deepest SoC sleep state reached |
| SELinux | ⚠️ Permissive | Enforcing planned before release |
| Recovery | ⚠️ Partly | 19.1 recovery: adb sideload does not work yet |

See [Known issues](known-issues.html) for details and [Changelog](changelog.html)
for progress.

## Before a release

- SELinux enforcing
- Remove bring-up debug options
- Working recovery for sideload
- Install guide and checksums
