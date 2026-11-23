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
| Display / smoothness | ✅ Works | SurfaceFlinger fix: app close animations no longer freeze |
| Wi-Fi | ✅ Works | Suspend filter for mDNS/SSDP improves standby |
| Bluetooth | ✅ Works | Two kernel crash fixes |
| Mobile (LTE, calls, SMS) | ✅ Works | Earpiece and loudspeaker |
| Camera (photos) | ✅ Works | Legacy camera HALv1 support restored |
| Camera (video recording) | ✅ Works | 720p and 1080p with sound |
| GPS | ✅ Works | Slow cold fix without assisted data |
| microSD | ✅ Works | vfat tested; exFAT not yet tested |
| Sensors | ✅ Works | Auto-brightness follows the room (new curve) |
| Charge limiter | ✅ Works | Quick Settings tile + Settings > Battery; heart battery icon while paused (in the next build) |
| Standby / Doze | ✅ Works | Deep Doze; tuning ongoing |
| SELinux | ⚠️ Permissive | Enforcing planned before release |
| Recovery | ⚠️ Partly | 19.1 recovery: adb sideload does not work yet |

See [Known issues](known-issues.html) for details and [Changelog](changelog.html)
for progress.

## Before a release

- SELinux enforcing
- Remove bring-up debug options
- Working recovery for sideload
- Install guide and checksums
