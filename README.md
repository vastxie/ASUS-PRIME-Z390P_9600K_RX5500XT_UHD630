# 黑苹果_华硕Z390P_i5-9600K_RX5500XT
[中文](https://github.com/vastxie/ASUS-PRIME-Z390-P_i5-9600K_RX5500XT/blob/main/README.md)  ｜ [English](https://github.com/vastxie/ASUS-PRIME-Z390-P_i5-9600K_RX5500XT/blob/main/README-EN.md)

参考[OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)，对`EFI`及`config.plist`做了精简和设置。

目前日常体验接近白苹果，暂未发现其他问题。

## 更新
+ 2021-01-10：升级[OpenCore](https://github.com/acidanthera/OpenCorePkg/releases)`0.6.5`和macOS`11.1`，使用一切正常。

## Tips
1. 机型默认设定为iMAC19.1，使用前建议自行生成三码并在`config.plist` -> `PlatformInfo` -> `Generic`中对应修改。（可使用OpenCore Configurator或 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)等生成）
   | 内容         | 对应位置                          |
   | ------------ | --------------------------------- |
   | Type         | `Generic` -> `SystemProductName`  |
   | Serial       | `Generic` -> `SystemSerialNumber` |
   | Board Serial | `Generic` -> `MLB`                |
   | SmUUID       | `Generic` -> `SystemUUID`         |
2. RX5500XT显卡原生驱动。此外在ACPI中添加了`SSDT-RX 5500 XT.aml`并配合`dAGPM.kext`驱动来优化显卡性能。
3. AirDrop & HandOff & Continuity 均能正常使用。
4. 有线网卡使用`RealtekRTL8111.kext`正常驱动。
5. EFI目录中标注了当前使用的驱动版本，可下载并在`./EFI/OC/Kexts/`中替换相关驱动进行升级。
6. 基于`Coffee Lake`架构的的黑苹果设备均可使用本EFI，根据核显及独显的不同使用，可能需要在`config.plist` -> `DeviceProperties` -> `Add` -> `PciRoot(0x0)/Pci(0x2,0x0)`相应调整。

## 硬件及驱动
| 配置      | 品牌                  | 型号                | 驱动                                                                                        |
| --------- | --------------------- | ------------------- | ------------------------------------------------------------------------------------------- |
| 主板      | 华硕-（ASUS）         | PRIME Z390-P        |                                                                                             |
| CPU       | 英特尔（Intel）       | i5-9600K            |                                                                                             |
| 显卡      | 蓝宝石（Sapphire）    | RX 5500 XT          | 原生支持 [WhateverGreen.kext](https://github.com/acidanthera/whatevergreen/releases) v1.4.6 |
| WiFi/蓝牙 | 奋威（Fenvi）         | FV-T919 BCM94360CD  | 免驱                                                                                        |
| 内存      | 美商海盗船(USCORSAIR) | DDR4 3600（8G * 4） |                                                                                             |
| 硬盘      | 三星（SAMSUNG）       | 970PRO 512G         |
| 板载声卡  | Realtek               | ALC887              | [AppleALC.kext](https://github.com/acidanthera/AppleALC/releases) v1.5.6                    |
| 板载网卡  | Realtek®              | RTL8111H            | [RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases) v2.3.0     |
| 传感器    |                       |                     | [VirtualSMC.kext](https://github.com/acidanthera/virtualsmc/releases) v1.1.9                |
| USB       |                       |                     | [USBInjectALL.kext](https://github.com/Sniki/OS-X-USB-Inject-All/releases) v0.7.0           |
| 其他      |                       |                     | [Lilu.kext](https://github.com/acidanthera/Lilu/releases) v1.5.0                            |

## Bios设置
BIOS版本：2808，加载默认设置后做了以下修改：
+ Intel(VMX)Virtualization Technology `Enabled`
+ 大于4G地址空间解码 `Enabled`
+ 初始化iGPU `Enabled`
+ Hyper M.2X16 `Enabled`
+ Serial Port `Disabled`
+ XHCI Hand-off `Enabled`
+ 快速启动 `Disabled`
+ 若出现错误等待按下F1键 `Disabled`

## EFI目录
```
.
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-AWAC.aml // 修复在较新硬件上的系统时钟。
    │   ├── SSDT-EC-USBX.aml // 修复嵌入式控制器和USB电源。
    │   ├── SSDT-PLUG.aml // CPU电源管理。
    │   ├── SSDT-PMC.aml // NVRAM支持。
    │   └── SSDT-RX 5500 XT.aml // 优化RX5500XT显卡性能。
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
    │   ├── WhateverGreen.kext // v1.4.6
    │   └── dAGPM.kext // v1.0.0
    ├── OpenCore.efi
    └── config.plist
```