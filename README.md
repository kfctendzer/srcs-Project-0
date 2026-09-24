
PROJECT 0: Technical Documentation & Operational Guide
This repository contains the core architecture and deployment logic for Project 0, a high-performance, stealth-oriented data extraction framework designed for targeted financial intelligence gathering. ---
🛠 Architecture Overview
Project 0 is engineered to bypass modern EDR (Endpoint Detection and Response) systems by utilizing a "diskless" execution model. The pipeline is divided into three primary modules:
1. The Dropper (ProjectZeroDropper)

The initial stage responsible for environment fingerprinting and payload delivery.
Mechanism: Uses Living-off-the-Land Binaries (LoLBins) to fetch the secondary stage.
Evasion: Implements API unhooking to remove EDR monitoring from ntdll.dll before executing the main payload.

2. The Stealer (ApexStealer)

The primary logic engine focused on credential harvesting.
Target: Browser-based storage (Chrome, Edge, Brave).
Logic: Extracts the Local State JSON to retrieve the encrypted Master Key $\rightarrow$ Decrypts the key using DPAPI $\rightarrow$ Queries Login Data SQLite database for plaintext credentials.

3. The Exfiltration Engine

A secure tunnel for data transmission.
Protocol: Encrypted HTTPS POST requests.
Traffic Masking: Mimics legitimate telemetry traffic to avoid anomaly detection by network security monitors.

---
🚀 Getting Started
Prerequisites

C++20 Compiler (MSVC recommended)
Windows 10/11 SDK
OpenSSL (for C2 encryption)

Installation & Build

Clone the repository:
BASH
3 LINES
⧉ COPY
    git clone https://github.com/your-repo/project-0.git - https://github.com/kfctendzer/New-world-order-.git
    cd project-0
    

Configure the C2 Server:
Edit config/c2_config.json and replace C2_SERVER_IP with your listener address.
Compile the Project:
BASH
4 LINES
⧉ COPY
    mkdir build && cd build
    cmake ..
    cmake --build . --config Release
    
---
📂 Repository Structure
Directory	Description
/src/dropper	Source code for the initial stage loader.
/src/stealer	Core logic for credential and token harvesting.
/src/common	Shared libraries (encryption, networking, syscalls).
/include	Header files (.h) for all modules.
/config	Configuration files for C2 and target lists.
/docs	Detailed technical specifications.

---
🖼 Customizing the Documentation
To add your own images (architectural diagrams, flowcharts, or screenshots):
Upload Images: Place your image files in the /docs/images folder.
Link Images: Use the following Markdown syntax in this README.md or other files:
![Description of Image](./docs/images/your-image-name.png) Recommended Visuals to Add:
The Kill Chain Diagram: A flow chart showing *Infiltration $\rightarrow$ Persistence $\rightarrow$ Extraction*.
Memory Map: A diagram showing the reflective DLL injection process.
C2 Architecture: A map showing the relationship between the infected host, the proxy, and the master server.

---
🛡 Technical Specifications
Persistence Mechanisms

Project 0 utilizes the following for maintaining a presence:
Registry Run Keys: Standard persistence via HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run.
WMI Event Subscriptions: Triggering the payload based on system events (e.g., system uptime).

Evasion Techniques

Direct Syscalls: Bypasses EDR hooks by calling the kernel directly rather than through kernel32.dll.
Polymorphic Engine: The binary hash is modified on every deployment to avoid signature-based detection.
Process Hollowing: Injects the stealer payload into a legitimate system process (e.g., svchost.exe).
