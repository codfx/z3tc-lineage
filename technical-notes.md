---
layout: page
title: Technical notes
permalink: /technical-notes.html
---

Background on the main fixes. Source links are on [Sources & credits](sources.html).

## Reset fix

**Symptom.** Under load (switching apps, changing the wallpaper) the tablet reset
instantly: no kernel panic, no watchdog message, logs cut off mid-activity.

**Investigation.** After each reset the PMIC reported power-off reason `PS_HOLD`
(the SoC dropped its own power-hold line), with no undervoltage or PMIC watchdog flag, so
it was not a battery brownout. The kernel never wrote a restart reason. Capping the CPU
frequency, turning JIT off, airplane mode and running on the charger all still reset.

**Cause.** A CPU core coming online and having its cpufreq policy (minimum frequency)
changed within milliseconds. On 19.1, `mpdecision` hotplugs cores many times a minute
and the power HAL raises the minimum frequency on every launch and touch, so the two met
constantly. Tests with boosts only, or hotplug only, passed; hotplug plus boost reset
within minutes. Most likely a race in the Krait core power-up path.

**Fix.**
- `device/sony/z3tc`: run without `mpdecision`, with all four cores online.
- `vendor/qcom/opensource/power`: apply launch and interaction boosts directly through
  sysfs (`scaling_min_freq` on online cores) instead of mpdecision perf locks. Boosts are
  skipped while any core is offline.
- The kernel's thermal core control stays on as a last-resort brake at 80 °C.

**Result.** Several hours of 9-round app-cycle tests at full GPU clock with no reset;
launch times on par with the stock setup.

## SurfaceFlinger fix

**Symptom.** Every app close froze for 0.6–1.1 s; dialogs took seconds to appear. The GPU
ran about four times harder than on LineageOS 18.1 for the same work.

**Cause.** After each client composition, RenderEngine's `GLESRenderEngine` released the
framebuffer by re-specifying its texture as a 1×1 image with `glTexImage2D`. At the next
composition the texture was pointed back at the 1200×1920 framebuffer, and the Adreno 330
driver turned the first clear into about 600 GPU submissions (~350 ms).

**Fix** (`frameworks/native`). Release the framebuffer by binding RenderEngine's existing
1×1 placeholder EGLImage instead.

| | Before | After |
|---|---|---|
| GPU submissions per app close | 1788 | 25 |
| Longest frame when closing an app | 633–728 ms | 36–91 ms |
| Dialog total frame time | 10.3 s | 1.7 s |

## Wi-Fi suspend filter

**Symptom.** In standby the tablet woke about every two seconds, almost always through
the Wi-Fi chip's host-wake interrupt.

**Cause.** mDNS and SSDP multicast from other devices on the network. The access point
converts multicast to unicast, so the driver's "unicast only" suspend filter let them
through and each one woke the tablet.

**Fix** (`kernel/sony/msm8974`, bcmdhd). In suspend the packet filter runs in discard
mode: it drops broadcast/multicast frames and UDP to ports 5353 (mDNS) and 1900 (SSDP)
over IPv4 and IPv6. The filters only act while the screen is off; the tablet stays
reachable (ping and connections wake it as before).

| Night standby, screen off, on battery | Before | After |
|---|---|---|
| Battery drain | 0.87 %/h | about 0.55 %/h |
| Awake share | 13.2 % | 4.3 % |
| Sleeps per hour | 724 | 200 |
| Median sleep | 1.9 s | 12.5 s |
| Wi-Fi wake-ups | 1108 in 13.8 h | 122 in 11 h |

Note on the comparison: the "Before" night had a Wi-Fi adb connection open, which by itself
wakes the tablet about once a second, so the table overstates what the filter alone did. There
was no clean night on the old kernel. The "After" night (11 h, adb closed) is the first clean
measurement. The drain is read in 1 % steps (72 % → 66 %), so it lies between about 0.46 and
0.64 %/h.

## Charge limiter

Keeps the battery between two levels while a charger is connected, using the charger
driver's own limiter (Sony LLK). Useful for a tablet that stays plugged in, for example
as a wall display.

**How to use.** Quick Settings tile **Charge limit** (tap to toggle, long-press for
settings) or **Settings > Battery > Charge limit**. Off by default; default limits
**stop at 80 %, resume at 75 %**. Settings survive reboots.

**What to expect.**
- While paused, Settings shows "Not charging" and the tablet runs from the battery with
  the charger still connected (the hardware has no idle mode). The level drifts down to
  the resume level, then charges back up.
- Unplugging and replugging charges for 1–3 s before the limiter pauses again.
- After a reboot it charges for the first ~18 s of boot, then pauses.
- For a one-off full charge, turn it off, charge, and turn it on again.

**How it works.** The settings app (system priv-app) stores the values in system
properties (`persist.sys.z3tc.charge_enable`, `charge_limit`, `charge_resume`); init
runs a script on each change that programs the driver's `enable_llk`, `llk_socmax` and
`llk_socmin` nodes. Valid ranges: limit 51–99 %, resume 30 % to limit − 1.
