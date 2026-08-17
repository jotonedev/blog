---
title:  "WindTre ZTE H389X techdata"
date:   2026-08-17T11:30:00+0100
image: /images/posts/2026-08-17-zte-h389x/external.webp
---

![Photo of the device](/images/posts/2026-08-17-zte-h389x/external.webp)

The ZTE H389X is a home router/Wi-Fi combo provided by the Italian ISP WindTre.

| Type | Chip | Notes |
|---|---|---|
| **CPU** | ZTE ZXIC ZX279132 | quad-core ARM Cortex-A53 @ 1.1 GHz (64-bit) |
| **RAM** | 512 MB | DDR3 |
| **Flash** | 512 MB NAND | HeYangTek HYF4GQ4UAACBE |
| **WLAN** | 2x Qualcomm QCN9000 | |
| **SFP slot** | ZTE ZX279132 PON controller | |

There is some work underway to port a different ZTE CPU to the Linux kernel to add OpenWrt support [here](https://gitlab.com/stefandoesinger/zx297520-kernel).

The software components used in the firmware that are subject to the GNU General Public License Version 2 or later are not publicly available on [opensource.ztedevices.com/](https://opensource.ztedevices.com/). Obtaining the source code requires sending an email to [tech.sp@zte.com.cn](mailto:tech.sp@zte.com.cn).

## Photos of the internals

![Photo of the device](/images/posts/2026-08-17-zte-h389x/front.webp)

![Photo of the device](/images/posts/2026-08-17-zte-h389x/back.webp)

## References

- https://orca.pet/wind3hub/ - Wind3 HUB notes (P7 firmware, U-Boot password, settings decryption)
- https://www.zteitalia.it/download/ - firmware download page
- https://www.acwifi.net/17312.html - another device with the same CPU
