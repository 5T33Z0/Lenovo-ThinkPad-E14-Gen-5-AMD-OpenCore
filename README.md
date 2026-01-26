# Lenovo ThinkPad E14 Gen 5 (AMD) Hackintosh OpenCore

<img width="2856" height="2376" alt="E14_Sequoia" src="https://github.com/user-attachments/assets/179d135e-0bd4-4aed-b8a7-74771fa3d7b2" />

## About

OpenCore EFI for running macOS Sequoia (and potentially Tahoe) on the Lenovo E14 Gen 5 with an AMD Ryzen 7 CPU. Early version, work in progress…

## Tech Specs

**Model**: [21JR002WGE](https://pcsupport.lenovo.com/de/de/products/laptops-and-netbooks/thinkpad-edge-laptops/thinkpad-e14-gen-5-type-21jr-21js/downloads)
**CPU**: AMD Ryzen 7 7730U (8 Cores/16 Threads)
**GPU**: AMD Radeon RX Vega 8 (4000/5000)
**RAM**: 16 GB (DDR4 SDRAM, PC4-25600, 3200 MHz)
**Storage**: 1 TB NVME (Western Digital PC SN740)

## What's working

- AMD CPU Power Management &rarr; Use AMD Power Gadget to adjust CPU behavior)
- AMD Radeon Graphics 2GB (via NootedRed kext) &rarr; Make sure to increase VRAM to at least 2 GB in BIOS (Config > Display > UMA Frame buffer Size)
- Audio (aclid=21 in boot-args). For some reason, injecting the alcid via DeviceProperties doesn't work
- HDMI

## What isn't working
- Networking and Bluetooth: MediaTek Wi-Fi 6 MT7921 Wireless LAN Card &rarr; Alternative: I use a USB WiFi Dongle ([**TL-WN725N**](https://www.tp-link.com/de/home-networking/adapter/tl-wn725n/)) and run it with Chris1111’s Tool [Wireless USB Big Sur Adapter](https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter) 
- Biometric: Goodix fingerprint
- Biometric: Facial Recognition (Windows Hello) Software Device

## macOS Install Notes
- Disable NootedRed kext during macOS install

## Post-Install Notes

### Install AMD Power Gadget
Install [**AMD Power Gadget**](https://github.com/trulyspinach/SMCAMDProcessor/releases) once macOS is up and running to monitor CPU Power Management

### Disable GateKeper (optional)
Only required if you want to use a WiFi USB Dongle and need to use Chris1111’s Tool [Wireless USB Big Sur Adapter](https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter) 

```shell
sudo spctl --master-disable
```

> [!IMPORTANT]
>
> In macOS Sequoia+, disabling Gatekeeper requires you to confirm the changes in System Settings &rarr; Gatekeeper &rarr; select "Allow apps from 'Everywhere'"

## Credits

Based on EFI by [op2025-ra](https://github.com/op2025-ra/ThinkPad-E14-Gen-5-Hackintosh) who also has an E14 Gen 5 but with a different CPU (6 Cores instead of 8)
