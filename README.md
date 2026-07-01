# Lenovo ThinkPad E14 Gen 5 (AMD) Hackintosh OpenCore

![](https://github.com/user-attachments/assets/36f1e6be-984b-46c6-82c5-3dbf9d0dba1b)

[![OpenCore](https://img.shields.io/badge/OpenCore-v1.0.8-cyan.svg?style=flat-square\&title=OpenCore%20Bootloader)](https://github.com/acidanthera/OpenCorePkg/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-15.7.x+-005BB5.svg?style=flat-square\&title=Supported%20macOS%20Versions)](https://www.apple.com/macos/)
[![Release](https://img.shields.io/badge/Download-Latest_Release-success.svg?style=flat-square\&title=Latest%20Release)](https://github.com/5T33Z0/Lenovo-ThinkPad-E14-Gen-5-AMD-OpenCore/releases)

## About

OpenCore EFI for running macOS on the Lenovo E14 Gen 5 with an AMD Ryzen 7 CPU. Tested with macOS Sequoia and Tahoe.

## Tech Specs

| Component   | Details | 
------------- |---------------------------------------------
| **Variant**   | [21JR002WGE](https://pcsupport.lenovo.com/de/de/products/laptops-and-netbooks/thinkpad-edge-laptops/thinkpad-e14-gen-5-type-21jr-21js/downloads) |
| **CPU**     | AMD Ryzen 7 [7730U](https://www.amd.com/de/products/processors/laptop/ryzen/7000-series/amd-ryzen-7-7730u.html) (8 Cores/16 Threads) 
| **iGPU**    | AMD Radeon RX Vega 8 (4000/5000)
| **RAM**     | 16 GB DDR4 SDRAM, PC4-25600, 3200 MHz
| **Storage** | 1 TB NVMe (Western Digital PC SN740)
| **WIFI/BT** | Intel® Wi-Fi 6E [AX210](https://www.intel.de/content/www/de/de/products/sku/204836/intel-wifi-6e-ax210-gig/specifications.html)*<br> **WiFi Fw**: `iwlwifi-ty-a0-gf-a0-68.ucode` + `iwlwifi-ty-a0-gf-a0.pnvm` <br> **BT Fw**: `ibt-0041-0041.sfi` + `ibt-0041-0041.ddc`

> [!NOTE]
> 
> *In stock configuration, this laptop variant comes with a MediaTek Wi-Fi 6 MT7921 card which is incompatible with macOS. So if you need Wi-Fi and Bluetooth, either upgrade your WiFi card or use a compatible USB WiFi dongle.

## What's Working

* [x] AMD CPU Power Management → install [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor/releases) to adjust CPU behavior
* [x] AMD Radeon Graphics 2GB (via NootedRed kext)
* [x] Audio*
* [x] HDMI Port
* [x] USB Port Mapping
* [x] Brightness and Volume controls via keyboard shortcuts
* [x] WiFi (via after-market WiFi/BT Card)
* [x] Slimed Intel Wifi and BT kexts to reduce overall EFI size

> [!NOTE]
> 
> When running macOS Tahoe, analog audio does not work out of the box. In this case, install [VoodooHDA-Tahoe](https://github.com/5T33Z0/Lenovo-ThinkPad-E14-Gen-5-AMD-OpenCore/raw/refs/heads/main/Assets/pkg/VoodooHDA-Tahoe.pkg) or apply root patches with [OCLP Mod](https://github.com/laobamac/OCLP-Mod/releases) in Post-Install.

### Notable Features

* Correct CPU name displayed in “About this Mac…”
* [GoldenGate Extended](https://github.com/HJebbour/GoldenGateExt-OpenCore-Theme?tab=readme-ov-file) icons by HJebbour

## What's Not Working

* [ ] Stock WiFi and Bluetooth: MediaTek Wi-Fi 6 MT7921 → incompatible. Alternative: USB WiFi Dongle ([TL-WN725N](https://www.tp-link.com/de/home-networking/adapter/tl-wn725n/)) with Chris1111’s [Wireless USB Big Sur Adapter](https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter) or ugrade the internal WiFi/BT Card
* [ ] Biometrics: Goodix fingerprint sensor and Windows Hello facial recognition

## Todos

- [ ] Enable proper Sleep/Hibernation

## Preparations

### BIOS Settings

Change the following settings to install/run macOS:

* Config > Display > UMA Frame buffer Size: 2G
* Security > Secure Boot > Off

### Config Adjustments

Before installing macOS, prepare your OpenCore EFI and configure it:

* Download the EFI folder from the Releases section
* **Unzip** it
* Open `config.plist` with [OCAT](https://github.com/ic005k/OCAuxiliaryTools) or your **preferred** plist editor
* **Adjust the following**:
  * `Kernel/Add`: If your System doesn’t use an Intel WiFi card, disable the Wifi and Bluetooth kexts, namely: itlwm, IntelBTPatcher, IntelBluetoothFirmware, BlueToolFixup
  * `Kernel/Patch`: If your E14 uses a difffernet CPU, adjust the Core Count (see [AMD-Vanilla Guide](https://github.com/AMD-OSX/AMD_Vanilla?tab=readme-ov-file#note-for-zen-4))
  * `PlatformInfo/Generic`: Generate Serial, ROM, MLB, etc.
* **Save** the `config.plist`

> [!IMPORTANT]
>
> When performing a first-time macOS installation or a clean/fresh install, make sure **NootedRed.kext** is disabled. If it remains enabled, the installer will hang at a later stage — specifically when the Setup Assistant appears to create a user account and complete initial configuration.

## macOS Installation

Once your EFI is ready:

* Put the EFI folder at the root of a FAT32-formatted USB flash drive. You can boot OpenCore from it
* To create a macOS USB installer, follow Dortania’s [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#making-the-installer)

## Post-Install Notes

### Disable Gatekeeper

Required for root patches and to run Chris1111’s WiFi USB tool:

```shell
sudo spctl --master-disable
```

> [!IMPORTANT]
>
> In macOS Sequoia+, confirm changes in System Settings → Gatekeeper → “Allow apps from Everywhere”

### Install HeliPort App (Intel Wi-Fi only)

Download, install and run [Heliport App](https://github.com/joshcalvert47/HeliPort) to be able to connect to Wi-Fi Access Points.

### Install AMD Power Gadget

Install [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor/releases) to monitor and adjust CPU power management

## Geekbench Scores

Componet | Score (Windows) | Score (macOS) 
-------- | ----------------|---------------
**CPU** | [Single-Core](https://browser.geekbench.com/v6/cpu/18552490): 1923 <br> [Multi-Core](https://browser.geekbench.com/v6/cpu/18552490): 5966 |
**GPU** | [Vulkan](https://browser.geekbench.com/v6/compute/6660369): 12659 <br>[OpenCL](https://browser.geekbench.com/v6/compute/6660375): 14608 

## Links
[Lenovo Driver and Software Matrix](https://download.lenovo.com/cdrt/tools/drivermatrix/dm_2.html)

## Credits

* Based on [op2025-ra](https://github.com/op2025-ra/ThinkPad-E14-Gen-5-Hackintosh); EFI cleaned and updated for current kexts, icons, and settings, plus macOS Tahoe support
* [AMD_Vanilla](https://github.com/AMD-OSX/AMD_Vanilla) – binary kernel patches for AMD CPUs
* ChefKissInc for NootedRed
* Fabiosun for helpful guidance
* Chris1111 fpr VoodooHDA-Tahoe
