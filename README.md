# Nyx: Genymotion ARM Translation for Android 11

## Overview
This guide explains how to enable ARM translation on Genymotion's Android 11 emulator using `libhoudini`. Tested on **Genymotion v3.8.0 on macOS Sequoia**, but it should work on other OS and recent Genymotion versions.

## Installation

### 1. Open the Emulator
Start your Genymotion Android 11 emulator.

### 2. Modify `build.prop`
To enable ARM support, edit `/system/build.prop` and `/system/vendor/build.prop`.

#### Step 1: Connect to the Emulator
```bash
adb shell
```

#### Step 2: Gain Superuser Access
```bash
su
```

#### Step 3: Remount the System Partition
```bash
mount -o rw,remount /
```

#### Step 4: Append the Following Lines to `build.prop`
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

#### Step 5: Exit the Shell
```bash
exit
```

### 3. Install `Nyx`
1. Download `Nyx` from the [releases page](https://github.com/).
2. Drag and drop `system.zip` onto the emulator. It will auto-install and prompt for a reboot.

### 4. Emulator Reboot
The emulator **will reboot automatically** after flashing `system.zip`. No manual restart needed.

## Verification
Run:
```bash
adb shell getprop ro.product.cpu.abilist
```
If you see `arm64-v8a, armeabi-v7a, armeabi`, it’s working. You can start using ARM APKs now.

## Troubleshooting
- If the emulator crashes, check that `build.prop` changes were saved.
- Ensure `/system` was properly remounted before editing.
- If `Nyx` doesn’t work, reflash `system.zip`.

## Conclusion
You’ve successfully enabled ARM translation on Genymotion’s Android 11 emulator, allowing ARM apps to run on x86-based virtual devices.

