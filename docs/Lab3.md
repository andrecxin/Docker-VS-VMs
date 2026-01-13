# Docker Android Emulator Lab

## Overview

This lab documents the process of running an Android emulator inside a Docker container using the `budtmo/docker-android` image. It includes steps to enable ADB connection, install APKs, and prepare the environment for dynamic application testing.

---

## 1. Setup Requirements

- Docker Desktop installed with WSL2 backend  
- KVM support enabled in your system  
- ADB (Android Debug Bridge) installed on host  
- `budtmo/docker-android` image pulled

---

## 2. Check KVM Support
Run the following command to verify that KVM acceleration is available on the host system.

```bash
kvm-ok

- Expected output:
INFO: /dev/kvm exists
KVM acceleration can be used

3. Run Docker Android Emulator
**Command**
docker run -d \
  -p 6080:6080 \
  -p 5555:5555 \
  -e EMULATOR_DEVICE="Samsung Galaxy S10" \
  -e WEB_VNC=true \
  --device /dev/kvm \
  --name android-container \
  budtmo/docker-android:emulator_11.0

**Ports**
6080 – Web VNC access via browser
5555 – ADB access port

Access the emulator in your browser at:
http://localhost:6080

4. Connect ADB
adb connect localhost:5555
adb devices

- Expected output
List of devices attached
localhost:5555    device

5. Install APK for Testing
Install the target APK onto the running emulator.
**Command**
adb install /mnt/c/Users/andre/Downloads/InsecureBankv2.apk

7. Notes
❌ This image does not include Google Play Services

✅ You can test custom or open-source vulnerable apps

⚠️ Performance depends on host machine's hardware
