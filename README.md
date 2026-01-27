# Lenovo ThinkPad E14 Gen 5 (AMD) Hackintosh OpenCore

![](https://github.com/user-attachments/assets/36f1e6be-984b-46c6-82c5-3dbf9d0dba1b)

[![OpenCore](https://img.shields.io/badge/OpenCore-v1.0.7-cyan.svg?style=flat-square&title=OpenCore%20Bootloader)](https://github.com/acidanthera/OpenCorePkg/releases/latest)
[![macOS](https://img.shields.io/badge/macOS-15.7.x+-005BB5.svg?style=flat-square&title=Supported%20macOS%20Versions)](https://www.apple.com/macos/)
[![Release](https://img.shields.io/badge/Download-Latest_Release-success.svg?style=flat-square&title=Latest%20Release)](https://github.com/5T33Z0/Lenovo-ThinkPad-E14-Gen-5-AMD-OpenCore/releases)

## About

OpenCore EFI for running macOS Sequoia on the Lenovo E14 Gen 5 with an AMD Ryzen 7 CPU. Trying to get Tahoe to work as well. Early version, work in progress…

## Tech Specs
- **Model**: [21JR002WGE](https://pcsupport.lenovo.com/de/de/products/laptops-and-netbooks/thinkpad-edge-laptops/thinkpad-e14-gen-5-type-21jr-21js/downloads)
- **CPU**: AMD Ryzen 7 [7730U](https://www.amd.com/de/products/processors/laptop/ryzen/7000-series/amd-ryzen-7-7730u.html) (8 Cores/16 Threads)
- **GPU**: AMD Radeon RX Vega 8 (4000/5000)
- **RAM**: 16 GB (DDR4 SDRAM, PC4-25600, 3200 MHz)
- **Storage**: 1 TB NVME (Western Digital PC SN740)

## What's working
- AMD CPU Power Management &rarr; install AMD Power Gadget to adjust CPU behavior
- AMD Radeon Graphics 2GB (via NootedRed kext) 
- Audio (aclid=21 in boot-args). For some reason, injecting the alcid via DeviceProperties doesn't work
- HDMI-Port
- USB Port Mapping
- WiFi – Requires USB Dongle

### Notable Features
- Correct CPU name displayed in "About this Mac…" section
- [GoldenGate Extended](https://github.com/HJebbour/GoldenGateExt-OpenCore-Theme?tab=readme-ov-file) Icons by HJebbour

### Todos
- Volume Control with Keyboard Shortcuts

## What isn't working
- Networking and Bluetooth: MediaTek Wi-Fi 6 MT7921 Wireless LAN Card &rarr; Alternative: I use a USB WiFi Dongle ([**TL-WN725N**](https://www.tp-link.com/de/home-networking/adapter/tl-wn725n/)) and run it with Chris1111’s Tool [Wireless USB Big Sur Adapter](https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter) 
- Biometric: Goodix fingerprint
- Biometric: Facial Recognition (Windows Hello) Software Device

## BIOS Settings
Change the following settings in order to be able to install/run macOS:

- Config > Display > UMA Frame buffer Size: 2G
- Security > Secure Boot > Secure Boot: Off

## macOS Install Notes
- Disable NootedRed kext during macOS install

## Post-Install Notes

### Install AMD Power Gadget
Install [**AMD Power Gadget**](https://github.com/trulyspinach/SMCAMDProcessor/releases) once macOS is up and running to monitor and adjust CPU Power Management

### Disable GateKeper (optional)
Only required if you want to use a WiFi USB Dongle and need to use Chris1111’s Tool [Wireless USB Big Sur Adapter](https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter) 

```shell
sudo spctl --master-disable
```

> [!IMPORTANT]
>
> In macOS Sequoia+, disabling Gatekeeper requires you to confirm the changes in System Settings &rarr; Gatekeeper &rarr; select "Allow apps from 'Everywhere'"

## Credits

Based on EFI by [op2025-ra](https://github.com/op2025-ra/ThinkPad-E14-Gen-5-Hackintosh) who also has an E14 Gen 5 but with a different CPU (6 Cores instead of 8) but improved.
