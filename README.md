# LSC Smart Indoor Camera: Security Research 

## 1. Overview
This repository documents a comprehensive security assessment of the LSC Smart Indoor Camera. The research involved physical firmware extraction, credential recovery, and the identification of multiple Remote Code Execution (RCE) vulnerabilities in the ONVIF service.

---

## 2. Technical Background & Environment

### 2.1 Firmware Acquisition
The initial analysis was performed by physically dumping the **SPI NOR Flash** from the device
. 
* **Hash Cracking:** Static analysis of the extracted `/etc/passwd` file revealed a DES-encrypted hash. After successful decryption, the root credentials were recovered: `root:dgiot010`.
* **Hardware Access:** Using these credentials, a persistent shell was established via the **UART console** pins on the board.

### 2.2 Attack Surface & Information Leaks
The `dgiot` binary, which manages the camera's core services, was found to contain several hardcoded secrets:
* **ONVIF Credentials:** `admin:12345678` (Enabled by default).
* **Unauthenticated RTSP Streams:** 
    * `rtsp://{IP}:8554/main`
    * `rtsp://{IP}:8554/sub`

---

## 3. Discovered Vulnerabilities

| CVE ID | Vulnerability Type | Component | Impact |
| :--- | :--- | :--- | :--- |
| **[CVE-2024-51347](./CVE-2024-51347.md)** | Stack Buffer Overflow | TimeZone Parameter | Remote Code Execution |
| **[CVE-2025-69986](./CVE-2025-69986.md)** | Stack Buffer Overflow | Protocol Parameter | Remote Code Execution |

---

