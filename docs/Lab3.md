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

```bash
kvm-ok

Expected output:
perl
Copy
Edit
INFO: /dev/kvm exists  
KVM acceleration can be used

3. Run Docker Android Emulator
bash
Copy
Edit
docker run -d \
  -p 6080:6080 \
  -p 5555:5555 \
  -e EMULATOR_DEVICE="Samsung Galaxy S10" \
  -e WEB_VNC=true \
  --device /dev/kvm \
  --name android-container \
  budtmo/docker-android:emulator_11.0

6080: Web VNC access via browser

5555: ADB access port

Access the emulator in your browser: http://localhost:6080

4. Connect ADB
bash
Copy
Edit
adb connect localhost:5555
adb devices
Expected output:

arduino
Copy
Edit
List of devices attached  
localhost:5555	device

5. Install APK for Testing
Example (path based on Windows):

bash
Copy
Edit
adb install /mnt/c/Users/andre/Downloads/InsecureBankv2.apk
Ensure the file path matches your actual APK location.

7. Notes
❌ This image does not include Google Play Services

✅ You can test custom or open-source vulnerable apps

⚠️ Performance depends on host machine's hardware