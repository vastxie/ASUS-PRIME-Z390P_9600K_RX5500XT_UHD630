# 黑苹果_华硕Z390P_i5-9600K_RX5500XT

OC EFI: ASUS PRIME Z390-P + i5-9600K Coffee Lake + RX5500/UHD 630

参考 [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)，对`EFI`及`config.plist`进行了修改和精简。

日常体验接近白苹果。

<details>

<summary>
更新日志
</summary>

- 2022-03-29：升级 Opencore 0.7.9，并更新相关驱动。
- 2021-11-03：升级 Opencore 0.7.5，更新驱动；升级 macOS 12.0.1 。
- 2021-09-16：升级 Opencore 0.7.3，更新驱动。
- 2021-08-07：升级 Opencore 0.7.2，更新驱动。
- 2021-07-07：升级 OpenCore 0.7.1，更新驱动，更新 macOS 版本 11.4。
- 2021-05-05：升级 OpenCore 0.6.9，更新驱动，更新 macOS 版本 11.3.1。
- 2021-04-15：精简`EFI`，添加`config-核显.plist`。
- 2021-04-11：升级 OpenCore 0.6.8，更新驱动。
- 2021-03-07：升级 OpenCore 0.6.7，更新驱动。
- 2021-02-05：升级 OpenCore 0.6.6，更新 macOS 11.2。
- 2021-01-16：定制 USB，添加`USBPorts.kext`补丁，修复无法使用 USB 3.0 设备的问题。
- 2021-01-12：删除`SSDT-RX 5500 XT.aml`和`dAGPM.kext`。
- 2021-01-10：升级 OpenCore 0.6.5 和 macOS 11.1。

</details>

## 使用指南

1. 默认机型为 iMAC19,1，需自行生成三码并在`config.plist` -> `PlatformInfo` -> `Generic`中对应修改。（可使用 OpenCore Configurator 或 GenSMBIOS 工具生成并修改）

2. 理论上基于 Coffee Lake 架构的 CPU 均可使用此`EFI`来引导启动黑苹果设备。可根据显卡使用情况自行选择`congfig.plist`文件。

   - 只使用核显需将`./EFI/OC/config_核显.plist`文件重命名为`config.plist`。
   - 若使用独显需将`./EFI/OC/config_独显.plist`文件重命名为`config.plist`。

3. 机箱不同可能出现一些USB接口无法使用的情况，可使用 [Hackintool](https://github.com/headkaze/Hackintool/releases) 工具定制USB驱动并替换`./EFI/OC/Kexts/USBPorts.kext`驱动文件。

## Bios设置

BIOS 版本：2808，加载默认设置后做了以下修改：

- Intel(VMX)Virtualization Technology [Disabled] -> [Enabled]
- 初始化 iGPU [Disabled] -> [Enabled]
- DVMT Pre-Allocated [64M] -> [128M]
- RC6(Render Srtandby) [Disabled] -> [Auto]
- Serial Port [Enabled] -> [Disabled]
- XHCI Hand-off [Disabled] -> [Enabled]
- 快速启动 [Enabled] -> [Disabled]
- 若出现错误等待按下 F1 键 [Enabled] -> [Disabled]

## 硬件

| 硬件        | 品牌/型号                                   |
| ----------- | ------------------------------------------- |
| 主板        | 华硕（ASUS） / PRIME Z390-P                 |
| CPU         | 英特尔（Intel） / i5-9600K                  |
| 显卡        | 蓝宝石（Sapphire） / RX 5500 XT             |
| WiFi / 蓝牙 | 奋威（Fenvi） / FV-T919 BCM94360CD          |
| 内存        | 美商海盗船(USCORSAIR) / DDR4 3600（8G * 4） |
| 硬盘        | 三星（SAMSUNG） / 970PRO 512G               |
| 板载声卡    | Realtek / ALC887                            |
| 板载网卡    | Realtek® / RTL8111H                         |

## EFI目录

```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-AWAC-DISABLE.aml // 修复在较新硬件上的系统时钟。
    │   ├── SSDT-PLUG.aml // CPU 电源管理。
    │   ├── SSDT-PMC.aml  // NVRAM 支持。
    ├── Drivers
    │   ├── OpenHfsPlus.efi
    │   └── OpenRuntime.efi
    ├── Kexts
    │   ├── AppleALC.kext //1.7.0
    │   ├── Lilu.kext //1.6.0
    │   ├── RealtekRTL8111.kext //2.4.2
    │   ├── SMCProcessor.kext 
    │   ├── SMCSuperIO.kext
    │   ├── USBPorts.kext //定制 USB 驱动
    │   ├── VirtualSMC.kext //1.2.9
    │   └── WhateverGreen.kext //1.5.8
    ├── OpenCore.efi
    ├── config-核显.plist
    └── config-独显.plist
```

## 更新OpenCore及驱动

1. 下载最新的 [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases)。

2. 预备一个新的启动介质（在 U 盘或硬盘新建`EFI`分区），挂载系统启动分区，复制旧`EFI`夹到新介质。

3. 替换需要更新的内容：

   - `EFI/BOOT/BOOTx64.efi`
   - `EFI/OC/OpenCore.efi`
   - `EFI/OC/Drivers/OpenRuntime`

4. 更新`EFI/OC/Kexts`中的驱动：

   - [AppleALC.kext](https://github.com/acidanthera/AppleALC/releases)
   - [Lilu.kext](https://github.com/acidanthera/Lilu/releases)
   - [RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)
   - [VirtualSMC.kext](https://github.com/acidanthera/virtualsmc/releases)
   - [WhateverGreen.kext](https://github.com/acidanthera/whatevergreen/releases)

5. `EFI/OC/config.plist`：比较`config.plist`和`sample.plist`并对应修改，确保配置符合最新的 OpenCore 版本。

6. 完成`EFI`修改后重启，在启动菜单栏按空格键，显示更多选项，选择`rest nvram`选项，系统自动重启后会更新 OC 版本号。

7. 确认更新的`EFI`可正常使用后，替换旧的`EFI`文件夹。
