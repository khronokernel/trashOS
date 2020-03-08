# HP Compaq DC7900 SFF


```
CPU:     Intel Core2 Quad Q8200
MOBO:    HP Compaq DC7900 SFF
RAM:     1GB(2x512MB 666Mhz)
GPU:     GT 710
NET:     Intel 82567LM
AUDIO:   AD1884A
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
* [AppleIntelE1000e](https://github.com/chris1111/AppleIntelE1000e/releases)


## DSDT Edits

Main thing to note with the HP DC7900 series is that they have some IRQ conflicts, specifically that the HPET and RTC will take hold of IRQs that is needed for USB, Ethernet and Audio to work. To get around this, we give HPET some `IRQNoFlags` to play with and remove the IRQ on RTC

I've also included a sample DSDT that's been patched, it'll work on BIOS ver. 1.16 but I highly recommend you **do not use as-is** unless you have the exact same BIOS with exact same hardware configuration


**HPET:**
```
Device (HPET)
{
    Name (_HID, EisaId ("PNP0103") /* HPET System Timer */)  // _HID: Hardware ID
    Name (_UID, One)  // _UID: Unique ID
    Name (CRES, ResourceTemplate ()
    {
        IRQNoFlags ()
            {3}
        IRQNoFlags ()
            {5}
        IRQNoFlags ()
            {7}
        IRQNoFlags ()
            {15}
        Memory32Fixed (ReadWrite,
            0xFED00000,         // Address Base
            0x00000400,         // Address Length
            )
    })
    Method (_CRS, 0, NotSerialized)  // _CRS: Current Resource Settings
    {
        Return (CRES) /* \_SB_.PCI0.LPC_.HPET.CRES */
    }

    Method (_STA, 0, NotSerialized)  // _STA: Status
    {
        Return (0x0F)
    }
}
```
**RTC:**
```
Device (RTC)
{
    Name (_HID, EisaId ("PNP0B00") /* AT Real-Time Clock */)  // _HID: Hardware ID
    Name (_CRS, ResourceTemplate ()  // _CRS: Current Resource Settings
    {
        IO (Decode16,
            0x0070,             // Range Minimum
            0x0070,             // Range Maximum
            0x00,               // Alignment
            0x02,               // Length
            )
    })
}
```
