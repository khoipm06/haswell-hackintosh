# Hasswell Hackintosh

## Overview

This repository contains the EFI folder and configuration for a Hackintosh build based on Haswell architecture. This setup allows running macOS on non-Apple hardware.

## Hardware Specifications

- **CPU**: Xeon E3-1230 v3 (Haswell)
- **Motherboard**: Asus H81M-K
- **RAM**: Doesn't matter, as long as it's compatible with the motherboard
- **GPU**: Nvidia GTX 650 (GK-107 variant), iGPU should work as well
- **Storage**: Kingspec 120G SSD
- **WiFi**: None wireless card installed
- **Bluetooth**: Every USB dongle should work as well as BlueToolFixup kext installed

## Features

- Full support for Haswell CPUs.
- Includes the GoldenGate theme from Acidanthera for better visual experience. You can change it as you like, such as tweaking the background image.
- Fully mapped USB for Asus H81M-K motherboard (includes back and front panel, mapped via USBPorts by Hackintool on macOS).
- Audio support via AppleALC.
- Ethernet support via RealtekRTL8111 kext.
- Sleep/Wake functionality works.
- AirDrop and iServices support with proper SMBIOS configuration.

> [!WARNING]
> - For most Haswell CPUs, this configuration should work without additional CPU-specific patches except for non-supported CPUs like Pentium and Celeron. You'll need further tweaks for those CPUs by emulating CPUID to a supported one.
>
> | Key       | Value                                      |
> |:----------|:-------------------------------------------|
> | Cpuid1Data | C0 06 03 00 00 00 00 00 00 00 00 00 00 00 00 00 |
> | Cpuid1Mask | FF FF FF FF 00 00 00 00 00 00 00 00 00 00 00 00 |
>
>   - My build uses Nvidia GTX 650 (GK-107 variant), which is Kepler-based, so no web drivers are needed and should work out of the box until macOS Big Sur. Currently, my EFI build supports up to macOS Monterey 12.7.6 although Apple had removed support for Kepler GPUs in macOS Monterey. You will need to use this [Geforce-Kepler-patcher](https://github.com/chris1111/Geforce-Kepler-patcher) or [other Kepler patch from OCLP](https://github.com/dortania/OpenCore-Legacy-Patcher) to bring back the removed kexts (also make sure to change your SMBIOS to iMac17,1 or higher to match the macOS version).
> - The reason why I deliberately emphasize on the GTX 650 GK-107 variant is that other GTX 650 variants (like GK106) have serious issues regarding VRAM leakage which cause distortions and instability in macOS, so it's not supported by those patches yet.
>     - Sadly, if you want to upgrade to a higher version, you will need to switch to a supported GPU like AMD RX 560 or higher, or disable the Nvidia GPU and use iGPU only.

> [!NOTE]
> - For iServices to work properly, ensure your serial number is unique and has been used on at least one real Apple device.
> - This motherboard is special when using ALC897, not the common ALC887. Be careful when choosing alcid for it.

## Not Working / Unsupported

- HDMI audio output (haven't tried it yet, my monitor only supports VGA).
- Native CPU power management (may work with further tweaks), so power consumption may be higher than expected compared to usage on Windows.
- Android/iOS tethering doesn't work out of the box (needs fixes).

## Installation Steps

Follow [Dortania's OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/) which is a very great resource on exploring about Hackintosh.

> [!TIP]
> - Make sure to adjust the SMBIOS settings in config.plist to match your desired macOS version and hardware.
> - Setup the BIOS/UEFI settings as per Dortania's recommendations.
> After copy the `OC` folder to your EFI partition, as most firmware won't auto-detect the OpenCore bootloader so you'll need to set it manually in BIOS/UEFI boot options. Or if your firmware doesn't support that, you can use tools like EasyUEFI/Bootice on Windows to add OpenCore as a boot option. On Linux you can use other tools to edit the NVRAM boot entries too, I would recommend using efibootmgr if you're familiar with cli.

## Post-Installation

- Use tools like Hackintool or IORegistryExplorer for troubleshooting.
- Enable iMessage/FaceTime by following [this guide](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html). Mostly by your improper SMBIOS settings. Moreover, your serial should be used by at least one real Apple device for the best chance of success.

## Troubleshooting

### Common Issues

- **Kernel Panic**: Check your config.plist for errors. Use OC Configurator or ProperTree.
- **No Audio**: Ensure AppleALC is configured correctly.
- **USB Issues**: Use USBPorts kext or USBMap kext. Follow the Dortania USB mapping guide.
- **Graphics Problems**: Adjust WhateverGreen properties.

## Credits

- [Dortania](https://dortania.github.io/) for OpenCore and guides.
- [Acidanthera](https://github.com/acidanthera) for kexts/tools and theme used in this build.
- [Chris1111](https://github.com/chris1111) for the Geforce Kepler patcher.

## Disclaimer

This is for educational purposes only. Ensure you have legal access to macOS. Hackintoshing may void warranties and is not officially supported by Apple.