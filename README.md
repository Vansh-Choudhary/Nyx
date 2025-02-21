# Nyx: Genymotion ARM Translation for Android 11

## Overview

Nyx is a tool that enables ARM translation on Genymotion's Android 11 emulator using libhoudini. This allows x86-based virtual devices to run ARM-based Android applications, significantly expanding the range of apps that can be tested and used in the emulator.

## What is libhoudini?

Libhoudini is an ARM translation layer developed by Intel and Google for Android. It allows x86 Android devices to run ARM-based applications by translating ARM instructions to x86 instructions on-the-fly. This technology is crucial for ensuring compatibility with a wide range of Android apps on x86 platforms.

## What is Houdini?

Houdini refers to the overall ARM translation system in Android, which includes libhoudini as its core component. It enables seamless execution of ARM binaries on x86 Android systems without requiring developers to recompile their apps specifically for x86 architecture.

## Features

- Enables ARM translation on Genymotion's Android 11 emulator
- Supports both 32-bit (armeabi-v7a) and 64-bit (arm64-v8a) ARM applications
- Seamless integration with the existing Android system
- Minimal performance overhead compared to full ARM emulation

## Installation

### Prerequisites

- Genymotion v3.8.0 or later (tested on macOS Sequoia, but should work on other OS)
- Android 11 emulator image
- ADB (Android Debug Bridge) installed and configured

### Steps

#### 1. Launch Emulator

Start your Genymotion Android 11 emulator.

#### 2. Modify `build.prop`

Connect to the emulator via ADB, gain root access, and modify system properties:

```bash
adb shell
```

```bash
su
```

```bash
mount -o rw,remount /
```

Append necessary configurations to enable ARM support:

```bash
echo 'ro.product.cpu.abilist=x86_64,x86,arm64-v8a,armeabi-v7a,armeabi
ro.product.cpu.abilist32=x86,armeabi-v7a,armeabi
ro.product.cpu.abilist64=x86_64,arm64-v8a
ro.vendor.product.cpu.abilist=x86_64,x86,arm64-v8a,armeabi-v7a,armeabi
ro.vendor.product.cpu.abilist32=x86,armeabi-v7a,armeabi
ro.vendor.product.cpu.abilist64=x86_64,arm64-v8a
ro.odm.product.cpu.abilist=x86_64,x86,arm64-v8a,armeabi-v7a,armeabi
ro.odm.product.cpu.abilist32=x86,armeabi-v7a,armeabi
ro.odm.product.cpu.abilist64=x86_64,arm64-v8a
ro.dalvik.vm.native.bridge=libhoudini.so
ro.enable.native.bridge.exec=1
ro.enable.native.bridge.exec64=1
ro.dalvik.vm.isa.arm=x86
ro.dalvik.vm.isa.arm64=x86_64
ro.zygote=zygote64_32' | tee -a /system/build.prop >> /system/vendor/build.prop
```

Exit the shell:

```bash
exit
```

#### 3. Install Nyx

- Download the latest `system.zip` from [here](https://github.com/Vansh-Choudhary/Nyx/releases/download/v1.0.0/system.zip).
- Drag and drop `system.zip` onto the running emulator.
- The system will automatically install and prompt for a reboot.

#### 4. Reboot

The emulator will automatically restart after installation.

## Verification

To verify successful installation, run:

```bash
adb shell getprop ro.product.cpu.abilist
```

If the output includes `arm64-v8a, armeabi-v7a, armeabi`, ARM translation is working correctly.

## Troubleshooting

- **Emulator Crashes**: Ensure `build.prop` changes were saved correctly.
- **System Partition Issues**: Verify `/system` was properly remounted before editing.
- **Nyx Installation Fails**: Try reflashing `system.zip`.

## How It Works

Nyx integrates libhoudini into the Genymotion Android 11 emulator by:

1. Modifying system properties to declare ARM ABI support.
2. Installing the libhoudini translation layer.
3. Configuring the Android runtime to use libhoudini for ARM code execution.

This allows the x86-based emulator to dynamically translate ARM instructions to x86, enabling seamless execution of ARM applications.

## Limitations

- Some performance overhead may be noticeable compared to native x86 execution.
- Not all ARM-specific hardware features can be emulated perfectly.

