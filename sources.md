---
layout: page
title: Sources & credits
permalink: /sources.html
---

## Source code

Every project with changes for this port is a public fork under
[github.com/codfx](https://github.com/codfx), branch **`z3tc-19.1`**, on top of LineageOS 19.1.

| Path | Repository |
|---|---|
| device/sony/z3tc | [android_device_sony_z3tc](https://github.com/codfx/android_device_sony_z3tc) (new device tree) |
| device/sony/shinano-common | [android_device_sony_shinano-common](https://github.com/codfx/android_device_sony_shinano-common) |
| device/sony/msm8974-common | [android_device_sony_msm8974-common](https://github.com/codfx/android_device_sony_msm8974-common) |
| kernel/sony/msm8974 | [android_kernel_sony_msm8974](https://github.com/codfx/android_kernel_sony_msm8974) |
| frameworks/native | [android_frameworks_native](https://github.com/codfx/android_frameworks_native) |
| frameworks/base | [android_frameworks_base](https://github.com/codfx/android_frameworks_base) |
| frameworks/av | [android_frameworks_av](https://github.com/codfx/android_frameworks_av) |
| packages/apps/Camera2 | [android_packages_apps_Camera2](https://github.com/codfx/android_packages_apps_Camera2) |
| vendor/qcom/opensource/power | [android_vendor_qcom_opensource_power](https://github.com/codfx/android_vendor_qcom_opensource_power) |
| lineage-sdk | [android_lineage-sdk](https://github.com/codfx/android_lineage-sdk) |
| packages/modules/adb | [android_packages_modules_adb](https://github.com/codfx/android_packages_modules_adb) |
| hardware/broadcom/wlan | [android_hardware_broadcom_wlan](https://github.com/codfx/android_hardware_broadcom_wlan) |
| hardware/qcom-caf/msm8974/display | [android_hardware_qcom_display](https://github.com/codfx/android_hardware_qcom_display) |
| hardware/interfaces | [android_hardware_interfaces](https://github.com/codfx/android_hardware_interfaces) |
| device/qcom/sepolicy-legacy | [android_device_qcom_sepolicy](https://github.com/codfx/android_device_qcom_sepolicy) |
| build/make | [android_build](https://github.com/codfx/android_build) |
| system/netd | [android_system_netd](https://github.com/codfx/android_system_netd) |
| system/bpf | [android_system_bpf](https://github.com/codfx/android_system_bpf) |

Unchanged dependencies come from LineageOS (Sony common, thermanager, timekeep,
macaddrsetup, Broadcom FM, legacy Qualcomm audio and media) and from marcogiannetta-pi
(`device/sony/z3c`).

### Building

1. Sync LineageOS 19.1 (`repo init -u https://github.com/LineageOS/android.git -b lineage-19.1`).
2. Add a local manifest that replaces the projects above with the codfx forks (branch
   `z3tc-19.1`) and adds the Sony/legacy Qualcomm dependencies. A ready-made manifest
   will be published with the first release.
3. Provide the proprietary vendor files (see below), then `breakfast z3tc` and `m bacon`.

### Proprietary files

Sony's proprietary vendor files (`vendor/sony/*`) are **not published** here. They have
to be extracted from your own device.

## Credits

- **marcogiannetta-pi** for the shinano LineageOS 19.1 (12L) trees for the Z3 Compact
  (device, common trees and kernel), which this port is built on, and for the USB adb fix
  from his newer shinano builds: the
  [legacy USB support for adbd](https://github.com/marcogiannetta-pi/android_device_sony_shinano-common/blob/lineage-22.2/patches/packages_modules_adb/0001-adb-Bring-back-legacy-USB-support-and-exclude-from-host.patch)
  and the FunctionFS/DWC3 parts of his
  [kernel commit 9a19a537](https://github.com/marcogiannetta-pi/android_kernel_sony_msm8974/commit/9a19a537a8154b52dc294e596aa536221e1e917b).
  See his repositories at [github.com/marcogiannetta-pi](https://github.com/marcogiannetta-pi).
- **The sony_scorpion LineageOS 18.1 z3tc device tree** (GitLab), the source of the
  tablet-specific files (audio, sensors, overlays, properties, kernel config).
- **The LineageOS project** and its contributors, including the former shinano and
  msm8974 maintainers whose trees and fixes made 19.1 on this platform possible.
- **AOSP** and the CAF/CodeLinaro legacy Qualcomm code.
- Port, z3tc device tree and fixes: **codfx**.
