# Docker and VM Testing Activities

This file documents the testing procedures and results from activities performed in both Docker containers and Virtual Machines (VMs), focusing on networking, scanning, and Android Studio usage. This serves as technical documentation for lab environments and can be added to MkDocs.

---

## 🧪 Test 1: Ping & Network Performance

### Objective

Compare basic connectivity and latency between Docker containers and VMs.

### Docker

* Containers connected using `bridge` and `labnet` networks.
* Ping between containers on custom bridge: average \~0.2 ms
* Ping external site (e.g., `8.8.8.8`): average \~1.2 ms

### VM

* Host: VirtualBox running Ubuntu 22.04
* Ping between host and VM: \~0.6 ms
* Ping to external: \~1.4 ms

### Notes

* Docker showed slightly faster inter-container ping.
* VMs had consistent latency but higher base resource usage.

---

## 🧪 Test 2: Vulnerability Scanning & Resource Usage

### Tools Used

* `wpscanteam/wpscan` Docker image
* WordPress site deployed via Docker Compose
* Scanned using:

  ```bash
  docker run -it --rm --network="bridge" wpscanteam/wpscan --url http://host.docker.internal:8080
  ```

### Docker

* Average scan time: 2.5 seconds
* Resource usage: \~215 MB RAM

### VM

* Same scan using WPScan CLI tool installed directly on Ubuntu VM
* Average scan time: \~3.8 seconds
* Resource usage: \~320 MB RAM

### Notes

* Docker faster due to more efficient networking and no GUI overhead.
* VM scan slightly slower but more flexible in terms of interactive debugging.

```md
| Test        | Docker | Virtual Machine |
|------------|--------|-----------------|
| Ping (avg) | 2 ms   | 7 ms            |
| RAM usage  | ~300MB | ~2GB            |

---

## 🧪 Test 3: Android Studio & Emulator

### Objective

Explore feasibility of running Android Studio inside Docker or VM.

### Docker Attempt

* Used image: `deadolus/android-studio-docker`
* Pulled successfully, but:

  * GUI forwarding and emulator setup is **not trivial**
  * Requires GPU passthrough or VNC setup
  * Limited hardware acceleration in Docker on Windows

### VM Test

* Used VirtualBox + Ubuntu 22.04
* Installed Android Studio + Emulator
* Emulator ran with hardware acceleration enabled
* Able to launch and test APKs with ADB

### Notes

* Android Studio in Docker is **experimental and unstable**.
* VM is more suitable for testing apps with real emulator support.

---

## 📌 Summary Table

| Test           | Docker            | VM                  |
| -------------- | ----------------- | ------------------- |
| Ping           | ✅ Fastest         | 🟡 Slightly slower  |
| WPScan         | ✅ Faster, low RAM | 🟡 Slower, more RAM |
| Android Studio | ❌ Not reliable    | ✅ Best option       |
| Web Emulator   | ✅ Can be reliable | ❌ Not working      |
