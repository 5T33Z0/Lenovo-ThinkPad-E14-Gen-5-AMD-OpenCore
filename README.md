# Lenovo ThinkPad E14 Gen 5 (AMD) Hackintosh OpenCore

![](https://github.com/user-attachments/assets/36f1e6be-984b-46c6-82c5-3dbf9d0dba1b)

[![OpenCore](https://img.shields.io/badge/OpenCore-v1.0.7-cyan.svg?style=flat-square\&title=OpenCore%20Bootloader)](https://github.com/acidanthera/OpenCorePkg/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-15.7.x+-005BB5.svg?style=flat-square\&title=Supported%20macOS%20Versions)](https://www.apple.com/macos/)
[![Release](https://img.shields.io/badge/Download-Latest_Release-success.svg?style=flat-square\&title=Latest%20Release)](https://github.com/5T33Z0/Lenovo-ThinkPad-E14-Gen-5-AMD-OpenCore/releases)

## About

OpenCore EFI for running macOS Sequoia on the Lenovo E14 Gen 5 with an AMD Ryzen 7 CPU. Tested with macOS Sequoia and Tahoe.

## Tech Specs

* **Model:** [21JR002WGE](https://pcsupport.lenovo.com/de/de/products/laptops-and-netbooks/thinkpad-edge-laptops/thinkpad-e14-gen-5-type-21jr-21js/downloads)
* **CPU:** AMD Ryzen 7 [7730U](https://www.amd.com/de/products/processors/laptop/ryzen/7000-series/amd-ryzen-7-7730u.html) (8 Cores / 16 Threads)
* **iGPU:** AMD Radeon RX Vega 8 (4000/5000)
* **RAM:** 16 GB DDR4 SDRAM, PC4-25600, 3200 MHz
* **Storage:** 1 TB NVMe (Western Digital PC SN740)

## What's Working

* AMD CPU Power Management → install [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor/releases) to adjust CPU behavior
* AMD Radeon Graphics 2GB (via NootedRed kext)
* Audio (use `alcid=21` in boot-args; DeviceProperties injection doesn’t work)
* HDMI Port
* USB Port Mapping
* Brightness and Volume controls via keyboard shortcuts

### Notable Features

* Correct CPU name displayed in “About this Mac…”
* [GoldenGate Extended](https://github.com/HJebbour/GoldenGateExt-OpenCore-Theme?tab=readme-ov-file) icons by HJebbour

## What's Not Working

* WiFi and Bluetooth: MediaTek Wi-Fi 6 MT7921 → incompatible. Alternative: USB WiFi Dongle ([TL-WN725N](https://www.tp-link.com/de/home-networking/adapter/tl-wn725n/)) with Chris1111’s [Wireless USB Big Sur Adapter](https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter)
* Biometrics: Goodix fingerprint sensor and Windows Hello facial recognition

## Todos

* Enable proper Sleep/Hibernation

## Preparations

### BIOS Settings

Change the following settings to install/run macOS:

* Config > Display > UMA Frame buffer Size: 2G
* Security > Secure Boot > Off

## Config Adjustments

Before installing macOS, prepare your OpenCore EFI and configure it:

* Download the EFI folder from the Releases section
* **Unzip** it
* Open `config.plist` with [OCAT](https://github.com/ic005k/OCAuxiliaryTools) or your **preferred** plist editor
* Adjust the following:
  * `Kernel/Add`: If you don’t use a Realtek USB WiFi dongle, disable the two `RtlWlanU` kexts
  * `Kernel/Patch`: If your CPU is different, adjust the Core Count (see [AMD-Vanilla Guide](https://github.com/AMD-OSX/AMD_Vanilla?tab=readme-ov-file#note-for-zen-4))
  * `PlatformInfo/Generic`: Generate Serial, ROM, MLB, etc.
* **Save** the `config.plist`

## macOS Installation

Once your EFI is ready:

* Put the EFI folder at the root of a FAT32-formatted USB flash drive. You can boot OpenCore from it
* To create a macOS USB installer, follow Dortania’s [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#making-the-installer)

## Post-Install Notes

### Disable Gatekeeper (optional but recommended)

Required for root patches and to run Chris1111’s WiFi USB tool:

```shell
sudo spctl --master-disable
```

> [!IMPORTANT]
>
> In macOS Sequoia+, confirm changes in System Settings → Gatekeeper → “Allow apps from Everywhere”

### macOS Tahoe

* Apply root patches with OCLP Mod to enable audio and USB WiFi dongle
* Settings screenshot:

  <img width="612" height="462" alt="oclp-mod" src="https://github.com/user-attachments/assets/07eb09dd-dda9-4815-bc97-7c0eac35edd5" />

### AMD Power Gadget

Install [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor/releases) to monitor and adjust CPU power management

## Credits

* Based on [op2025-ra](https://github.com/op2025-ra/ThinkPad-E14-Gen-5-Hackintosh); EFI cleaned and updated for current kexts, icons, and settings, plus macOS Tahoe support
* [AMD_Vanilla](https://github.com/AMD-OSX/AMD_Vanilla) – binary kernel patches for AMD CPUs
* ChefKissInc for NootedRed
* Fabiosun for helpful guidance
