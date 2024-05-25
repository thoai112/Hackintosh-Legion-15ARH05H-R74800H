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

### Known Issues

1. Opening certain applications may cause screen artifacts, freezes, or crashes.

   > Related: [Advanced OpenGL apps may have artefacts or freeze the system · Issue #158 · ChefKissInc/NootedRed (github.com)](https://github.com/ChefKissInc/NootedRed/issues/158)

   - Applications with issues: Chrome, Edge, Notion

   - Problem: The graphics driver [NootedRed](https://github.com/NootInc/NootedRed) is not compatible with the new version of OpenCL used by these applications, waiting for the graphics driver to be updated and fixed.

   - Temporary solutions:

     - Disable hardware acceleration in the settings of the problematic application

     - (Not recommended by the driver author) Introduce [BFixup.kext](https://github.com/ChefKissInc/NootedRed/issues/158#issuecomment-1842011907), downgrade the OpenGL of the graphics driver

     - (Recommended) Use the terminal to start the App and add startup parameters (disable GPU compositing UI)
       Example:

       ```shell
       open -a "Microsoft Edge.app" --args --disable-gpu-compositing
       ```

       > Tip: You can package this command with added startup parameters into a new `.app` application using Automator.
       
     - For Firefox browser, the above `BFixup.kext` and startup parameters are ineffective, you can use [old version Firefox 79.0](https://archive.mozilla.org/pub/firefox/releases/) (Thanks to [@yaxirhuxxain](https://github.com/jimlee2048/Hackintosh-Lenovo-Legion-R7000P2020H/issues/1#issuecomment-1935373999))

2. Opening certain applications may cause lag.

   - Solution: Use [UMAF](https://github.com/DavidS95/Smokeless_UMAF) to manually set the video memory to 2G

3. Video hardware decoding is not available

   > Related: [Image & Video hardware decoding/encoding is dysfunctional (github.com)](https://github.com/ChefKissInc/NootedRed/issues/28)

   - Graphics driver issue, no solution for now, waiting for updates and fixes.

4. The system occasionally fails to start and gets stuck somewhere.

   - Temporary solution: Try to restart after forcibly shutting down by holding down the power button. If it doesn't work, try to force restart several times.
   - Solution: Disable XHC0 (ACPI-> check `SSDT-XCH0-DISABLE.aml`) & Do not use the modified XHCI driver (Kext-> uncheck `GenericUSBXHCI`).
     - Note: This solution will disable 2 USB ports (top C port & left A port), please choose wisely.

5. Occasionally after system startup, the keyboard/touchpad cannot be used normally.

   - Solution: Restart the system.

6. No matter how you restart the system, the touchpad does not work.

   - Your touchpad is likely not made by Synaptic, so it is not compatible with the currently used default touchpad driver [VoodooRMI.kext](https://github.com/VoodooSMBus/VoodooRMI).
   - Please try to switch to the standard I2C touchpad driver, the specific operation is as follows: Edit the OpenCore boot configuration (`EFI/OC/config.plist`), disable the driver shown in the red box under Kernel > Add, and enable the driver shown in the blue box.
     ![image](https://github.com/jimlee2048/Hackintosh-Lenovo-Legion-R7000P2020H/assets/21117946/c1271daa-7666-49f3-9480-f090e8fd434f)

7. After waking up from sleep, Bluetooth may stop working and cannot connect to any device.

   - Wait a while after waking up, observe whether it can automatically reconnect after 1-2min, sometimes it can automatically recover.
   - If not, you can try to manually turn Bluetooth on and off.
   - If Bluetooth still does not work after trying to manually turn it on and off, then restart the system.
   - Replacing with a common white Apple wireless card can effectively alleviate this problem.

8. After a long sleep, the screen is black and cannot be awakened normally, but the system can still work normally (you can hear the keyboard prompt sound)

   > Related: [Black screen after a long sleep, but the system works · Issue #213 · ChefKissInc/NootedRed (github.com)](https://github.com/ChefKissInc/NootedRed/issues/213)

   - Graphics driver issue, no solution for now, please force restart.

9. Deep sleep (hibernate) is not available, and after deep sleep restart, it may cause black/blue/screen artifacts and freeze.

   - No fix has been found yet, please turn off deep sleep mode.

     ```shell
     sudo pmset -a hibernatemode 0
     sudo pmset -a autopoweroff 0
     sudo pmset -a standby 0
     ```

## Acknowledgements

- [Apple](https://www.apple.com/) for macOS.
- [acidanthera](https://github.com/acidanthera) for developing OpenCore and a large number of important kexts.
- [NootedInc](https://chefkissinc.github.io/) for developing NootedRed, adapting VoodooI2C for AMD, and the help from member [@VisualEhrmanntraut](https://github.com/VisualEhrmanntraut).
- [W2725730722](https://github.com/W2725730722/Lenovo-R7000P-2020-Hackintosh) for the excellent work that this repository is based on, continuing to evolve and modify the provided Sonoma EFI.
-[jimlee2048](https://github.com/jimlee2048/Hackintosh-Lenovo-Legion-R7000P2020H) continuing to evolve and modify the provided Sonoma 14.4 EFI.
- [Dortania](https://dortania.github.io/), [daliansky/OC-little](https://github.com/daliansky/OC-little), [5T33Z0/OC-Little-Translated](https://github.com/5T33Z0/OC-Little-Translated) and others for the detailed and comprehensive tutorials and guides.
- And all the friends in the Hackintosh community who love to tinker and share, wishing everyone a fun time :)