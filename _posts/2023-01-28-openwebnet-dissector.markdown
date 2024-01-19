---
layout: post
title:  "An OpenWebNet dissector for Wireshark"
subtitle: "I recently noted there is no dissector yet for the protocol OpenWebNet. So, I developed one."
date:   2023-01-28 16:11:44 +0100
image:
    path: /assets/images/openwebnet_dissector.png
    alt: screenshot of the dissector in action in Wireshark
---
## What is OpenWebNet

[OpenWebNet](https://en.wikipedia.org/wiki/OpenWebNet) is a protocol developed by Bticino used for controlling their MyHome system. With this protocol, you can send commands to gateways or _touchscreens_ (touchscreens are just hidden gateways with also more features). What they do is translate the OpenWebNet messages to signals on the [SCS Bus](https://en.wikipedia.org/wiki/Bus_SCS) and the other way around.

## Architecture of Gateways

All gateways run on some sort of Linux distribution. The first models use an Intel Processor with the instruction set x86-32. All the other usually use an arm processor of some sort. I also find noteworthy to mention that they (the company) don't respect the GPL license and they don't provide the source code of the kernel on their website.

## Wireshark Dissector

![Dissector](https://github.com/jotonedev/openwebnet-dissector/raw/main/screenshot/01.png)

I developed the dissector in Lua using the provided [documentation](https://web.archive.org/web/20230127151350/https://developer.legrand.com/documentation/open-web-net-for-myhome/) by Legrand.
Also, I've noticed that there are many undocumented features, like the older authentication system used by gateways prior 2016. Even if there is no documentation, here you can find the implementation of the algorithm in Python: [github.com/karel1980/ReOpenWebNet/password.py](https://github.com/karel1980/ReOpenWebNet/blob/abb027cdb6c708e85b8f1fe710e5f4cc867be02b/src/reopenwebnet/password.py).

The dissector: [github.com/jotonedev/openwebnet-dissector](https://github.com/jotonedev/openwebnet-dissector)
