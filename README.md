# trashOS

This repo is mainly used as a getting started with page with Core2 series hardware running on OpenCore. There's also some sample EFIs for a few hardware configurations I've ran with:

* [HP DC7900 SFF](/HP-Compaq-DC7900)
* [Dell Studio 540](/Dell-Studio-540)

## Install process

1. Get macOS: [GibmacOS](https://github.com/corpnewt/gibMacOS)
2. Build installer: [`createinstallmedia`](https://support.apple.com/en-us/HT201372)
3. Enable legacy booting: [Legacy Install](https://dortania.github.io/OpenCore-Desktop-Guide/extras/legacy)
4. Build rest of installer
5. Boot and install as usual
6. Mojave and newer users: You'll also want to grab the [telemetrap.kext](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/post-28447707) to fix the SSE4,2 requirement


## OpenCore Specifics

**config.plist**:

* SMBIOS: `iMac10,1` or `iMac13,2`
   * Depends on the OS, Mojave+ doesn't support iMac10,1
   * Note macOS Big Sur removed iMac14,3 and older, so will require iMac14,4 minimum
* FadtEnableReset: `True`
   * Needed for proper shutdown/restarts
* RebaseRegions: `True`
   * Only needed when running a custom DSDT like with the HP DC7900
* Vault: `Optional`
   * Required for booting unless you've sugned OpenCore yourself
* ScanPolicy: `0`
   * To show all drives at the OpenCore picker

Besides that, everything else works as stock

**Firmware Drivers(.efi)**:

* [FwRuntimeServices](https://github.com/acidanthera/OpenCorePkg/releases)
   * Helps with Native NVRAM and other misc. things
* [AppleUsbKbDxe](https://github.com/acidanthera/OpenCorePkg/releases)
   * Needed for the picker
* [ApfsDriverLoader](https://github.com/acidanthera/AppleSupportPkg/releases)
   * Needed for seeing APFS Volumes
* [HfsPlusLegacy](https://github.com/acidanthera/OcBinaryData)
   * Needed for seeing HFS volumes
* [ExFatDxeLegacy](https://github.com/acidanthera/OcBinaryData)
   * Needed for seeing drives made by Bootcamp assistant

**SSDTs**:

No specific SSDTs required to boot, though SSDT-EC is recommended for loading AppleBusPowerController on Mojave and older

**Kexts**:

The main ones are as follows:

* [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases) **or** FakeSMC
* [Lilu](https://github.com/acidanthera/Lilu/releases)
* [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)
* [AppleALC](https://github.com/acidanthera/AppleALC/releases) **or** [VoodooHDA](https://sourceforge.net/projects/voodoohda/)
* [telemetrap.kext](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/post-28447707)

Ethernet gets a bit more complicated as we're going into the depths of legacy hackintosh kexts, so support on Catalina can be a bit sketchy:

* [AppleIntelE1000e](https://github.com/chris1111/AppleIntelE1000e/releases)
* [IntelMausi](https://github.com/acidanthera/IntelMausi/releases)
* [RealtekRTL8100](https://github.com/Mieze/RealtekRTL8100)
* [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)
* [AtherosE2200Ethernet](https://github.com/Mieze/AtherosE2200Ethernet/releases)
