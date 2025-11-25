# Gigabyte Z590 Vision G Hackintosh

OpenCore EFI configuration for Gigabyte Z590 Vision G running macOS Sequoia.

![about-this-mac](https://github.com/user-attachments/assets/e49dc317-a366-45ba-a903-632ccad180bc)

## Hardware Specifications

| Component | Model |
|-----------|-------|
| **CPU** | Intel Core i9-11900 |
| **GPU** | SAPPHIRE NITRO+ AMD Radeon RX 6900 XT SE |
| **RAM** | G.SKILL Trident Z Neo DDR4-3600MHz CL18 64GB (2x32GB) |
| **Motherboard** | Gigabyte Z590 Vision G |
| **Storage** | WD Black SN850 NVMe SSD |
| **Audio** | Realtek ALC4080 |
| **Ethernet** | Intel X710 Dual SFP+ (PCIe) |
| **WiFi/BT** | Fenvi T919 (BCM94360CD) |
| **BIOS** | F2 |

## Software Versions

- **macOS**: Sequoia
- **OpenCore**: 1.0.4
- **SMBIOS**: iMac20,2

## What Works

- GPU Graphics Acceleration
- WiFi and Bluetooth
- AirPlay Audio and Video
- Ethernet (Intel X710 Dual SFP+)
- Audio (Rear + Front)
- Sleep and Wake
- AirDrop
- iMessage, FaceTime, App Store
- Handoff
- USB Ports (All mapped)
- Hardware Acceleration

## What Doesn't Work

Everything works as expected!

## ACPI SSDTs

| SSDT | Description |
|------|-------------|
| **SSDT-AWAC.aml** | Fixes RTC-related boot issues |
| **SSDT-EC-USBX.aml** | Fake Embedded Controller, fixes USB power |
| **SSDT-RHUB.aml** | Disables RHUB device to force manual USB port rebuild |
| **SSDT-UIAC-Z590-VISION-G-V3.aml** | Custom USB port map (15 ports) |

## Kexts Used

### Essential
- **Lilu.kext** (1.7.0) - Patching framework
- **VirtualSMC.kext** (1.3.4) - SMC emulation
- **WhateverGreen.kext** (1.6.9) - GPU patching
- **NVMeFix.kext** (1.1.2) - NVMe power management

### Sensors
- **SMCProcessor.kext** - CPU temperature monitoring
- **SMCSuperIO.kext** - Fan speed monitoring
- **SMCRadeonSensors.kext** (2.3.1) - AMD GPU temperature monitoring

### Networking
- **IntelLucy.kext** - Intel X710 Ethernet support
- **SmallTreeIntel8259x.kext** - Additional Ethernet support for Sequoia
- **AirportBrcmFixup.kext** - BCM94360CD WiFi support
- **IO80211FamilyLegacy.kext** - Legacy WiFi support for Sequoia
- **IOSkywalkFamily.kext** - Network stack for Sequoia

### USB
- **USBInjectAll.kext** (0.7.8) - USB port injection

### Other
- **AMFIPass.kext** - AMFI patching for Sequoia
- **EnergyDriver.kext** - Power management

## Boot Arguments

```
agdpmod=pikera revpatch=sbvmm
```

- `agdpmod=pikera` - Disables board ID checks on AMD Navi GPUs
- `revpatch=sbvmm` - Required for root patching on Sequoia

## BIOS Settings

### Disable
- Fast Boot
- Secure Boot
- Serial/COM Port
- VT-d (can be enabled if you set `DisableIoMapper` to YES)
- CSM
- Intel SGX
- Intel Platform Trust
- CFG Lock (or use `AppleCpuPmCfgLock` / `AppleXcpmCfgLock` quirks)

### Enable
- VT-x
- Above 4G Decoding
- Hyper-Threading
- Execute Disable Bit
- EHCI/XHCI Hand-off
- OS Type: Windows 8.1/10 UEFI Mode
- DVMT Pre-Allocated (iGPU Memory): 64MB or higher (iGPU disabled in this build)

## Notable Configuration

### CPU Spoofing
This build uses CPU ID spoofing to emulate Comet Lake (0x0A0671) for compatibility, as Rocket Lake CPUs require this for macOS.

### GPU Configuration
- iGPU is disabled in BIOS
- Using dedicated AMD RX 6900 XT
- Native AMD GPU support in macOS (no additional patches needed beyond `agdpmod=pikera`)

### USB Mapping
Custom USB port mapping via SSDT-UIAC limits to 15 ports as per macOS requirements.

## Installation Notes

1. Generate your own SMBIOS data using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
2. Update the following in `config.plist`:
   - `PlatformInfo > Generic > MLB`
   - `PlatformInfo > Generic > SystemSerialNumber`
   - `PlatformInfo > Generic > SystemUUID`
3. Update BIOS to F2 or later
4. Configure BIOS settings as listed above
5. Disable iGPU in BIOS (or configure iGPU + dGPU if needed)

## Screenshots

![system-info](https://github.com/user-attachments/assets/526fc9b4-bcf0-4171-a973-3d3d1f477781)

![gpu-info](https://github.com/user-attachments/assets/c1e34336-73cd-4baa-a0f2-3fbf6e674fe6)

## Credits

- [Acidanthera](https://github.com/acidanthera) for OpenCore and most kexts
- [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [r/hackintosh](https://www.reddit.com/r/hackintosh/) community

## Disclaimer

This EFI is provided as-is for educational purposes. Hackintosh is not supported by Apple. Use at your own risk.
