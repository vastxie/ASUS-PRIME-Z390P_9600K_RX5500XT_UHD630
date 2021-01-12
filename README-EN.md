# Hackintosh_AsusZ390P_i5-9600K_RX5500XT
[中文](https://github.com/vastxie/ASUS-PRIME-Z390-P_i5-9600K_RX5500XT/blob/main/README.md)  | [English](https://github.com/vastxie/ASUS-PRIME-Z390-P_i5-9600K_RX5500XT/blob/main/README-EN.md)

Reference [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/). Treamline and setting in `EFI` and `config.plist`.

The daily experience is close to Apple Macintosh, and no other problems have been found.

Geekbench 5 `Single-Core Score: 1259` `Multi-Core Score: 5681` `OpenCL Score: 41933`

## Update
+ 2020-01-11: Change the model to `iMAC20,2`. Delete `SSDT-RX 5500 xt.aml` and `dagpm.kext`. 
+ 2021-01-10: Update [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases)`0.6.5` and macOS`11.1`. Everything works fine.

## Tips
1. The default setting of the model is ~~`iMAC19,1`~~ `iMAC20,2`. Pleade generrate the three finger by yourself before use.
   | PART         | COPY TO                           |
   | ------------ | --------------------------------- |
   | Type         | `Generic` -> `SystemProductName`  |
   | Serial       | `Generic` -> `SystemSerialNumber` |
   | Board Serial | `Generic` -> `MLB`                |
   | SmUUID       | `Generic` -> `SystemUUID`         |
2. ~~RX5500XT display native driver. In addition, `SSDT-RX 5500 xt.aml` has been added in ACPI and the driver `dagpm. kext` has been used to optimize the performance of the dGPU.~~
3. AirDrop & HandOff & Continuity can be used normally.
4. The wired network card is normally driven by `RealtekRTL8111.kext`.
5. The current driver vesion has been marked in EFI directory. you can download and replace the relevant driver in `./EFI/OC/Kexts/` to upgrade.

## Hardware and Driver
| Type           | Brand     | Item                | Driver                                                                                  |
| -------------- | --------- | ------------------- | --------------------------------------------------------------------------------------- |
| Motherboard    | ASUS      | PRIME Z390-P        |                                                                                         |
| CPU            | Intel     | i5-9600K            |                                                                                         |
| Video Card     | Sapphire  | RX 5500 XT          | [WhateverGreen.kext](https://github.com/acidanthera/whatevergreen/releases) v1.4.6      |
| WiFi/Bluetooth | Fenvi     | FV-T919 BCM94360CD  | native driver                                                                           |
| Memory         | USCORSAIR | DDR4 3600（8G * 4） |                                                                                         |
| Storage        | SAMSUNG   | 970PRO 512G         |
| Network Card   | Realtek   | ALC887              | [AppleALC.kext](https://github.com/acidanthera/AppleALC/releases) v1.5.6                |
| Sound Card     | Realtek®  | RTL8111H            | [RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases) v2.3.0 |
| Sensor         |           |                     | [VirtualSMC.kext](https://github.com/acidanthera/virtualsmc/releases) v1.1.9            |
| USB            |           |                     | [USBInjectALL.kext](https://github.com/Sniki/OS-X-USB-Inject-All/releases) v0.7.5       |
| Other          |           |                     | [Lilu.kext](https://github.com/acidanthera/Lilu/releases) v1.5.0                        |

## Bios Setting
Bios version：2808.
The following modifications were made after loading the default.
+ Intel(VMX)Virtualization Technology `Enabled`
+ Above 4G Decoding `Enabled`
+ iGPU Multi-Monitor `Enabled`
+ Hyper M.2X16 `Enabled`
+ Serial Port `Disabled`
+ XHCI Hand-off `Enabled`
+ Fast Boot `Disabled`
+ Wait For 'F1' If Error `Disabled`

## EFI Directory
```
.
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-AWAC.aml // Fixed system clock on newer hardware.
    │   ├── SSDT-EC-USBX.aml // Restore the embedded controller and USB power supply.
    │   ├── SSDT-PLUG.aml // CPU Power Management.
    │   └── SSDT-PMC.aml // NVRAM Support.
    ├── Drivers
    │   ├── HfsPlus.efi
    │   └── OpenRuntime.efi
    ├── Kexts
    │   ├── AppleALC.kext // v1.5.6
    │   ├── Lilu.kext // v1.5.0
    │   ├── RealtekRTL8111.kext // v2.3.0
    │   ├── SMCProcessor.kext // v1.1.9
    │   ├── SMCSuperIO.kext // v1.1.9
    │   ├── USBInjectAll.kext // v0.7.5
    │   ├── VirtualSMC.kext // v1.1.9
    │   └── WhateverGreen.kext // v1.4.6
    ├── OpenCore.efi
    └── config.plist
```
