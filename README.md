# Python Remote Administration Tool (RAT) PoC

![Python](https://img.shields.io/badge/Python-3.x-blue.svg)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey)
![License](https://img.shields.io/badge/License-Educational-orange)

## 📖 Overview

This project is a cross-platform Remote Administration Tool (RAT) implementation developed to study offensive security concepts, specifically Command & Control (C2) architecture and system persistence. It demonstrates how Python can be used to interact with low-level OS APIs and maintain secure, persistent connections across different operating environments.

**Disclaimer:** *This software is for educational purposes only. It is designed to help security researchers and developers understand how malware operates to better defend against it. Do not use this on systems you do not own.*

## 📂 Project Structure

- **`RAT/Plain_RAT.py`**: A clean, documented implementation of the implant. Best for understanding the core logic and control flow.
- **`RAT/Obfuscated_RAT.py`**: An experimental variant implementing anti-analysis techniques, obfuscation, and evasion logic.
- **`listener.py`**: The server-side component (C2) that accepts connections and issues commands to the agent.
- **`Transfer_tool.py`**: Utility for handling file transfer operations.

## 🚀 Key Features

### 1. Cross-Platform Persistence
The tool automatically detects the underlying OS and installs persistence using native mechanisms:
- **Windows:** Registry modification (`HKCU...Run`).
- **Linux:** XDG Autostart (`.desktop` files).
- **macOS:** LaunchAgents (`.plist` files).

### 2. Encrypted Communication
- Implements a custom application-layer protocol on top of TCP.
- Uses **SSL/TLS wrapping** for encrypted transport, preventing network sniffing of commands and output.
- Custom packet structure (4-byte length header + JSON payload) ensures data integrity.

### 3. Remote Capabilities
- **Shell Access:** Execute system commands and retrieve stdout/stderr.
- **File Management:** Upload and download files securely.
- **Surveillance:** Capture screenshots on Windows, Linux, and macOS.
- **Reconnaissance:** Dead Drop Resolution (fetching C2 IP from a public source like Pastebin) to avoid hardcoded IPs.

## 🛠️ Installation & Usage

### Prerequisites
- Python 3.x
- Required packages: `requests`, `Pillow`, `pyautogui` (install via `pip install -r requirements.txt` if available).

### Running the C2 Server
Start the listener on your attacking machine:
```bash
python3 listener.py
```

### Deploying the Agent
On the target machine (for testing):
```bash
# For clean version
python3 RAT/Plain_RAT.py

# For obfuscated version
python3 RAT/Obfuscated_RAT.py
```

*Note: In a real-world simulation, these scripts would typically be compiled into standalone executables using tools like PyInstaller.*

## 🛡️ Security & Detection
This project serves as a case study for Blue Teams. Detection opportunities include:
- **Network:** Monitoring for non-standard SSL traffic or connections to known paste sites.
- **Host:** Watching for modifications to Registry Run keys or the creation of hidden files in user directories.
- **Behavior:** Flagging processes spawning shells (`cmd.exe`, `/bin/sh`) or taking periodic screenshots.

## 📝 License
This project is open-source for educational use. See `SECURITY.md` for full policy details.