# HP Compaq DC7900 SFF


```
CPU:     Intel Core2 Duo E8500
MOBO:    Dell Studio 540
RAM:     8GB 800MHz(2GBx4)
AUDIO:   ALC888S
GPU:     GT 710
NET:     Realtek RTL8168
BOOT:    OpenCore
```

## What works/doesn't work

Works:

* Catalina
* Ethernet
* Audio
* Hardware Acceleration
* DRM(AppleTV+, Netflix works in Chrome)
* Startup Disk/NVRAM
* iMessage

Doesn't work:

* Sleep
   * fixed if you setup an SSDT-CPUPM
* PS2 keyboard/Mouse
   * VoodooPS2 fixes this, haven't bothered to try
* On-board iGPU
   * 4500 HD is unsupported regardless of macOS version

## Kexts

These are just the essential ones to get booting and have proper functionality 

* [Lilu](https://github.com/acidanthera/Lilu/releases)
* [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)
* [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)
* [VoodooHDA](https://sourceforge.net/projects/voodoohda/)
* [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)
* [telemetrap.kext](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/post-28447707)
