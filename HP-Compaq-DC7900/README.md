# HP Compaq DC7900 SFF

```
CPU:     Intel Core2 Quad Q8200
MOBO:    HP Compaq DC7900 SFF
RAM:     8GB(4x2GB 800MHz)
GPU:     GT 710
NET:     Intel 82567LM
AUDIO:   AD1884A
BOOT:    OpenCore
```

## What works/doesn't work

Works:

* Tested versions of macOS:
  * OS X 10.5.0 (9a581)
  * OS X 10.6.0 (10a432) and 10.6.7 (10J4139)
  * OS X 10.7.0 (11A511)
  * OS X 10.8.5 (12F37)
  * OS X 10.9.5 (13F34)
  * OS X 10.10.5 (14F27)
  * OS X 10.11.6 (15G31)(Only tested installer)
  * macOS 10.12.6 (16G29)
  * macOS 10.13.6 (17G11023)
  * macOS 10.14.0 (18A391)
  * macOS 10.15.4 (19E242d)
* Ethernet
* Audio
* Hardware Acceleration
* DRM(AppleTV+, Netflix works in Chrome)
* Startup Disk/NVRAM
* iMessage

Doesn't work:

* Sleep
   * Fixed if you setup an SSDT-CPUPM
* PS2 keyboard/Mouse
   * VoodooPS2 fixes this, haven't bothered to try
* On-board iGPU
   * 4500 HD is unsupported regardless of macOS version

## Kexts

These are just the essential ones to get booting and have proper functionality:

* [Lilu](https://github.com/acidanthera/Lilu/releases)
* [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)
* [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)
* [VoodooHDA](https://sourceforge.net/projects/voodoohda/)
* [AppleIntelE1000e](https://github.com/chris1111/AppleIntelE1000e/releases)
* [telemetrap.kext](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/post-28447707)

I've also set MaxKernels set to allow legacy booting with 32bit based kexts.

## ACPI Hot Patches

Due to IRQ issues, you'll need the following:

* [SSDT-CSR-HPET](/HP-Compaq-DC7900/0.6.1 HP EFI/EFI/OC/ACPI/SSDT-CSR-HPET.aml)
* CRES to XCRES Patch under ACPI -> Patch:
  * Find:      `48504554085F4849440C41D00103085F5549440A010843524553`
  * Replace:   `48504554085F4849440C41D00103085F5549440A010858524553`


