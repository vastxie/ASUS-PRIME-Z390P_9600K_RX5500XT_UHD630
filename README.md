# 黑苹果_华硕Z390P_i5-9600K_RX5500XT

参考[OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)，对`EFI`及`config.plist`做了精简和设置。

本`EFI`适用于使用`Coffee Lake`架构的`CPU`，并使用`RX5000`系列显卡作为独显或不使用独显仅使用`Intel UHD Graphics 630`核显的设备。

AirDrop & HandOff & Continuity 均能正常使用。
日常体验接近白苹果，暂未发现其他问题。

## 更新

+ 2021-04-15：精简`EFI`,添加`config-核显.plist`。
+ 2021-04-11：升级 OpenCore`0.6.8`，更新驱动到最新版本。
+ 2021-03-07：升级 OpenCore`0.6.7`及最新驱动。
+ 2021-02-05：升级 OpenCore`0.6.6`和macOS`11.2`。
+ 2021-01-16：定制USB，添加`USBPorts.kext`补丁，修复无法使用USB3.0设备的问题。
+ 2021-01-12：删除`SSDT-RX 5500 XT.aml`和`dAGPM.kext`。
+ 2021-01-10：升级 OpenCore`0.6.5`和macOS`11.1`，使用一切正常。

## 使用指南

1. 默认机型为`iMAC19,1`，需自行生成三码并在`config.plist` -> `PlatformInfo` -> `Generic`中对应修改。（可使用 OpenCore Configurator 或 GenSMBIOS 等生成）
   | 内容         | 对应位置                          |
   | ------------ | --------------------------------- |
   | Type         | `Generic` -> `SystemProductName`  |
   | Serial       | `Generic` -> `SystemSerialNumber` |
   | Board Serial | `Generic` -> `MLB`                |
   | SmUUID       | `Generic` -> `SystemUUID`         |
2. 理论上基于`Coffee Lake`架构的`CPU`均可使用此`EFI`来引导启动黑苹果设备。
   + 默认`config.plist`为核显用于计算，使用`RX5000`系列显卡驱动显示屏。
   + 若不使用独显仅使用核显，需用`./EFI/OC/config-核显.plist`替换默认的`config.plist`文件。
3. 不同机箱可能出现一些USB接口无法使用的情况，可使用[Hackintool](https://github.com/headkaze/Hackintool/releases) 工具定制USB驱动并替换 `./EFI/OC/Kexts/USBPorts.kext`。

## 硬件及驱动

表中标注了当前使用的驱动版本，可在`./EFI/OC/Kexts/`中替换相关驱动进行升级。

| 配置      | 品牌                  | 型号                | 驱动                                                                                        |
| --------- | --------------------- | ------------------- | ------------------------------------------------------------------------------------------- |
| 主板      | 华硕-（ASUS）         | PRIME Z390-P        |                                                                                             |
| CPU       | 英特尔（Intel）       | i5-9600K            |                                                                                             |
| 显卡      | 蓝宝石（Sapphire）    | RX 5500 XT          | 原生支持 [WhateverGreen.kext](https://github.com/acidanthera/whatevergreen/releases) v1.4.9 |
| WiFi/蓝牙 | 奋威（Fenvi）         | FV-T919 BCM94360CD  | 免驱                                                                                        |
| 内存      | 美商海盗船(USCORSAIR) | DDR4 3600（8G * 4） |                                                                                             |
| 硬盘      | 三星（SAMSUNG）       | 970PRO 512G         |
| 板载声卡  | Realtek               | ALC887              | [AppleALC.kext](https://github.com/acidanthera/AppleALC/releases) v1.5.9                    |
| 板载网卡  | Realtek®              | RTL8111H            | [RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases) v2.4.0     |
| 传感器    |                       |                     | [VirtualSMC.kext](https://github.com/acidanthera/virtualsmc/releases) v1.2.2                |
| 其他      |                       |                     | [Lilu.kext](https://github.com/acidanthera/Lilu/releases) v1.5.2                            |

## Bios设置

BIOS版本：2808，加载默认设置后做了以下修改：

+ Intel(VMX)Virtualization Technology [Disabled]->[Enabled]
+ 初始化iGPU [Disabled]->[Enabled]
+ DVMT Pre-Allocated [64M]->[128M] 
+ RC6(Render Srtandby) [Disabled]->[Auto]
+ Serial Port [Enabled]->[Disabled]
+ XHCI Hand-off [Disabled]->[Enabled]
+ 快速启动 [Enabled]->[Disabled]
+ 若出现错误等待按下F1键 [Enabled]->[Disabled]

## EFI目录

```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-AWAC-DISABLE.aml // 修复在较新硬件上的系统时钟。
    │   ├── SSDT-PLUG.aml // CPU电源管理。
    │   ├── SSDT-PMC.aml  // NVRAM支持。
    ├── Drivers
    │   ├── OpenHfsPlus.efi
    │   └── OpenRuntime.efi
    ├── Kexts
    │   ├── AppleALC.kext
    │   ├── Lilu.kext
    │   ├── RealtekRTL8111.kext
    │   ├── SMCProcessor.kext
    │   ├── SMCSuperIO.kext
    │   ├── USBPorts.kext //定制USB驱动
    │   ├── VirtualSMC.kext
    │   └── WhateverGreen.kext
    ├── OpenCore.efi
    ├── config-核显.plist
    └── config.plist
```

## 如何更新OpenCore

1. 下载[OpenCore](https://github.com/acidanthera/OpenCorePkg/releases)。
2. 预备一个新的启动介质（在U盘或硬盘新建EFI分区），挂载系统启动分区，复制旧EFI文件夹到新介质。
3. 需要更新的内容：
   + `EFI/BOOT/BOOTx64.efi`
   + `EFI/OC/OpenCore.efi`
   + `EFI/OC/Drivers/OpenRuntime`
4. 更新`EFI/OC/Kexts`中的驱动。
5. 更新`EFI/OC/config.plist`：比较`config.plist`和`sample.plist`，确保配置符合最新版本的 OpenCore。
6. 确认更新的EFI可正常使用后，替换旧的EFI文件夹。
