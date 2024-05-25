# Hackintosh for Lenovo Legion 15ARH05H R7-4800H


## Introduction

Pre-configured OpenCore EFI for Lenovo Legion 5 15ARH05H - R74800H - (2020 model).

| Item                 | Info                                                       |
| -------------------- | ---------------------------------------------------------- |
| Opencore Version     | 0.9.9                                                      |
| Model                | Lenovo Legion Legion 5 15ARH05H - R74800H (2020)<br />联想拯救者R7000P（2020款）    |
| SMBIOS used          | MacBookPro16,3                                             |
| Target MacOS Version | macOS Sonoma 14.5                                          |

This EFI may also be compatible with the following models, please test the availability yourself:

- Lenovo Legion R7000P2020H : Sharing same bios firmware with Legion 5-15ARH05H, most likely compatible.
- Lenovo Rescuer R7000 (2020 model)

## Availability

Status ID:

- 🟢=Available
- 🟡=Available but with problems (see below for details on the problem)
- 🔴=Not available

### Hardware

| Item       | Info                        | Status                                                     | Notes                                                        |
| ---------- | --------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| CPU        | AMD Ryzen™ 7 4800H          | 🟢                                                          | Power management & sensor information display： [AMDRyzenCPUPowerManagement](https://github.com/trulyspinach/SMCAMDProcessor) + [SMCAMDProcessor](https://github.com/trulyspinach/SMCAMDProcessor) |
| Display       | AMD Radeon™ Vega 8          | 🟡 Hardware acceleration<br />🔴 Video hard explanation<br />🔴 Video output (HDMI, C port DP)  | Driver：[NootedRed](https://github.com/NootInc/NootedRed)<br />Sensor information display：[RadeonSensor](https://github.com/aluveitie/RadeonSensor) + [SMCRadeonGPU](https://github.com/aluveitie/RadeonSensor)<br />Video output independent display passthrough，NoHope|
| GPU       | Nvidia Geforce RTX 2060 6GB | 🔴                                                          |use [Bumblebee Method](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/laptop-disable.html#bumblebee-method)禁用，`SSDT-dGPU-OFF-NoHybGfx.aml` |
| Wireless network card   | Intel® Wi-Fi 6 AX200        | 🟢 WIFI<br />🟡 Bluetooth                                        | drive：<br />[IntelBluetoothFirmware.kext + IntelBTPatcher](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/pull/446) (Use the latest PR to fix the problem that some LE devices cannot connect.) <br />+ [AirportItlwm-Sonoma](https://github.com/OpenIntelWireless/itlwm/releases/tag/v2.3.0-alpha)<br />+ [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM?tab=readme-ov-file#bluetoolfixupkext) |
| Ethernet   | Realtek RTL8111             | 🟢                                                          | Driver：[RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X) |
| Audio      | Realtek ALC257              | 🟢 Sound card<br />🟢 Built-in speaker<br />🟢 Built-in microphone<br />🟢 Headphone jack | Driver: AppleALC, use [layout-id 101](https://github.com/acidanthera/AppleALC/blob/master/Resources/ALC257/Info.plist)<br /> |
| Keyboard   |                             | 🟢 Keyboard<br />🟢 Backlight control<br />                             | Driver：[VoodooPS2Controller](https://github.com/acidanthera/VoodooPS2) |
| Touchpad | Synaptics `SYNA0000`        | 🟢 Interrupt mode<br />🟢 Multi-finger/gesture recognition<br />🟢 Anti-accidental touch detection          | Driver： [VoodooRMI](https://github.com/VoodooSMBus/VoodooRMI) (Mode I2C) + [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C/pull/532) (Use v2.9 adapted to AMD) |
| USB        | XHC0<br />XHC1              | 🟡                                                          | All USB interfaces are available, including C port, up to USB3.2 Gen1 speed<br />Driver ：[GUX-RyzenXHCIFix](https://github.com/RattletraPM/GUX-RyzenXHCIFix) (AMD Ryzen dedicated magic modification) |
| Power/battery  |                             | 🟢                                                          | Battery information display： [SMCBatteryManager.kext](https://github.com/acidanthera/VirtualSMC)<br /> |
| Screen   | 15.6' 1080P  144Hz          | 🟢 144Hz high refresh rate<br />🟢 Screen backlight brightness control                        |                                                              |

### Features

| Item     | Status                                                       | Notes                                                        |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Sleep    | 🟢 Enter/exit sleep by closing/opening the lid<br />🟡 Sleep<br />🔴 Deep Sleep (Hibernate, write to disk & power off) | Current optimization measures:<br />- Introduced [CpuTscSync](https://github.com/Seey6/CpuTscSync) (Modified version, optimized for AMD mobile processors, to improve sleep death problem)<br />- Upgrade BIOS version to FSCN28WW (to solve various strange sleep problems, please operate by yourself)<br />- Turn off deep sleep (see below for introduction) |
| Lenovo fn key | 🟢 Perfectly supported (status display & software control)<br />- F1-F4&Home-PgDn Audio control<br />- F5-F6 Screen brightness control<br />- F8 Airplane mode<br />- F10 Touchpad control<br />🟡 Normally usable (can be triggered normally)<br />- fn+Q Mode switch (cannot be controlled by software, no trigger status display)<br />- fn+Space Keyboard backlight control (can be controlled by software, no trigger status display)<br />- fn+Esc Fn lock (can be controlled by software, no trigger status display)<br />🔴 Not usable: Other unlisted fn keys | Control driver & matching software: [YogaSMC](https://github.com/zhen-zen/YogaSMC) |
| Apple services | Only list the unavailable:<br />🔴 AirDrop, Handoff, Universal Control<br />🔴 Cross-device sync of Focus status/Screen Time<br />🔴 Some Apps: Home, iMessage, FaceTime<br />🔴 Sidecar | The current Intel wireless card driver is not compatible.<br />It can be used after replacing with a common white Apple wireless card (except for Sidecar, waiting for the graphics card driver to fix hardware decoding)<br /> |

