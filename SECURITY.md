# Security Policy & Technical Architecture

## ⚠️ Legal Disclaimer & Ethical Notice

**This repository is for educational purposes and authorized security research only.**

This software is a Proof of Concept (PoC) Remote Administration Tool (RAT) designed to demonstrate:
1.  Network socket programming and encrypted communication.
2.  Operating System internals and persistence mechanisms.
3.  Evasion techniques against static analysis.

**Usage:**
- The code provided herein should **never** be used on systems without explicit, written permission from the owner.
- The author denies all responsibility for any misuse or damage caused by this software.
- This project is intended to help security professionals and students understand how C2 (Command & Control) infrastructure operates to better defend against it.

---

## 🔬 Technical Overview

This project explores the implementation of a cross-platform implant written in Python, capable of maintaining access and executing commands on Windows, Linux, and macOS environments.

### 1. Network Communication
The agent establishes a reverse TCP connection to a hardcoded C2 server (or one dynamically resolved via a Dead Drop Resolver, e.g., Pastebin).
- **Protocol:** TCP/IP sockets wrapped in SSL context.
- **Encryption:** `ssl.CERT_NONE` (Self-signed certificate compatibility).
- **Data Transport:** JSON-serialized payloads with a 4-byte big-endian length header to ensure stream integrity.

### 2. Persistence Mechanisms
The tool identifies the host OS at runtime and employs native techniques to survive system reboots:

| Operating System | Technique | Implementation Detail |
|------------------|-----------|----------------------|
| **Windows** | Registry Run Keys | Modifies `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` via `winreg`. |
| **Linux** | XDG Autostart | Injecting `.desktop` entries into `~/.config/autostart/`. |
| **macOS** | LaunchAgents | Creating `com.apple.systemupdate.plist` in `~/Library/LaunchAgents/`. |

### 3. Evasion & Anti-Analysis (Experimental)
The `backdoor2.py` variant demonstrates defensive evasion concepts:
- **Dynamic Imports:** Uses `__import__` to obscure dependency analysis.
- **String Obfuscation:** Critical strings (IPs, URLs) are Base64 encoded to bypass basic grep/string scanning.
- **Heuristic Evasion:** Implements timing checks and "garbage code" execution to confuse sandbox environments and heuristic engines.
- **Environment Checks:** Detects specific environment variables typically found in analysis sandboxes.

### 4. Capabilities
- **File System:** Remote upload/download and directory navigation.
- **Execution:** Shell command execution via `subprocess`.
- **Surveillance:** Cross-platform screenshot capture using `PIL` or `pyautogui`.

---

## 🐛 Reporting Vulnerabilities

As this is a research project, standard vulnerability reporting does not apply in the traditional sense. However, if you identify:
1.  Remote Code Execution (RCE) vulnerabilities in the *listener* component.
2.  Cryptographic weaknesses in the transport layer (beyond the known use of self-signed certs).

Please open an issue detailing the remediation steps.
