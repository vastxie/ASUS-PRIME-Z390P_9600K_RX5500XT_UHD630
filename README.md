# ASUS-PRIME-Z390-P_i5-9600K_RX5500XT
黑苹果OpenCore EFI分享: 华硕Z390P + i5-9600K + RX5500XT

参考[OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)，对EFI及config.plist做了精简和设置。

本人日常电脑基本不关机，不用的时候关掉显示屏，目前日常体验非常完美。若在使用中遇到问题，欢迎在Issues中与我交流反馈。

## 更新
+ 2021-01-10：升级OpenCore 0.6.5和macOS 11.1，使用一切正常。

## 硬件配置
1. 主板：华硕（ASUS）PRIME Z390-P
2. CPU：英特尔（Intel）i5-9600K
3. 显卡：蓝宝石（Sapphire）RX 5500 XT
4. WiFi/蓝牙：Fenvi FV-T919 BCM94360CD
5. 内存：美商海盗船(USCORSAIR) DDR4 3600（8G * 4）
6. 硬盘：三星（SAMSUNG）970PRO 512G

## Bios设置
BIOS版本：2808，加载默认设置后做了以下修改：
+ Intel(VMX)Virtualization Technology [Enabled]
+ 大于4G地址空间解码 [Enabled]
+ 初始化iGPU [Enabled]
+ Hyper M.2X16 [Enabled]
+ Serial Port [Disabled]
+ XHCI Hand-off [Enabled]
+ 快速启动 [Disabled]
+ 若出现错误等待按下F1键 [Disabled]

## Tips
1. 机型默认设定为iMAC19.1，使用前建议自行生成三码并在config.plist -> PlatformInfo -> Generic中对应修改。（可使用OpenCore Configurator或 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)等生成）
   | 内容         | 对应位置                      |
   | ------------ | ----------------------------- |
   | Type         | Generic -> SystemProductName  |
   | Serial       | Generic -> SystemSerialNumber |
   | Board Serial | Generic -> MLB                |
   | SmUUID       | Generic -> SystemUUID         |
2. 显卡原生驱动。此外在ACPI中添加了SSDT-RX 5500 XT.aml并配合dAGPM.kext驱动来释放显卡性能。
3. AirDrop & HandOff & Continuity 均能正常使用。
4. 有线网卡使用RealtekRTL8111.kext正常驱动。

## EFI目录
```
.
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-AWAC.aml
    │   ├── SSDT-EC-USBX.aml
    │   ├── SSDT-PLUG.aml
    │   ├── SSDT-PMC.aml
    │   └── SSDT-RX 5500 XT.aml
    ├── Drivers
    │   ├── HfsPlus.efi
    │   └── OpenRuntime.efi
    ├── Kexts
    │   ├── AppleALC.kext
    │   ├── Lilu.kext
    │   ├── RealtekRTL8111.kext
    │   ├── SMCProcessor.kext
    │   ├── SMCSuperIO.kext
    │   ├── USBInjectAll.kext
    │   ├── VirtualSMC.kext
    │   ├── WhateverGreen.kext
    │   └── dAGPM.kext
    ├── OpenCore.efi
    └── config.plist
```
