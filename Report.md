# Static Malware Analysis Report: `sample.elf`

This repository contains the deliverables for **Task 7: Static Malware Analysis**, a part of the Cyber Security Internship. The goal of this task was to analyze a suspicious file without executing it to determine its functionality, origin, and key Indicators of Compromise (IOCs).

## 🔬 Analysis Methodology

The analysis was performed in a controlled, isolated Virtual Machine (VM) running Kali Linux to ensure the malware sample posed no risk to the host machine. The following tools were used to conduct a multi-phase static inspection:

* `file` & `sha256sum`/`md5sum`: For initial file identification and hashing.

* VirusTotal: For a quick reputation check and global context.

* `strings`: To extract readable content from the binary.

* Ghidra: A software reverse engineering (SRE) tool for deep code analysis and decompilation.

## 📝 Analysis Findings

### 1. File Identification and Triage

| Identifier | Value |
| :--- | :--- |
| **File Name** | `sample.elf` |
| **File Type** | ELF 32-bit LSB executable, ARM, **statically linked, stripped** |
| **SHA-256 Hash** | `bcf3f55866d6efe185e0154feecad5808f4e6b7d80898a85e93f667d84e04` |
| **MD5 Hash** | `50f25d01b3b24f38ddb3743198b55a3bf` |
| **Compiler** | GCC (GNU) 4.1.2 |

_The file's **statically linked** nature and the "stripped" compiler flag indicate a deliberate attempt to make analysis more difficult by removing function names and embedding all library code within the binary itself._

### 2. Suspicious Strings (`strings` output)

Extraction of readable strings revealed clear intent for network communication and system control, common in botnet malware. The full output is available in `suspicious_strings.txt`.

* `HTTP/1.1\r\nUser-Agent:` - Indicates network communication via HTTP.

* `/proc/net/tcp` & `/proc/net/status` - Used to check system network connections.

* `/var/condiBot` & `/var/zxrcr9999` - Suspicious file paths, likely for persistence or configuration.

* `/usr/bin/reboot` & `/usr/bin/shutdown` - System control commands.

* `/usr/bin/wget` & `/usr/bin/curl` - Used to download additional payloads.

### 3. Ghidra Decompilation and Logic Flow

The file's entry point and functions were obfuscated, with no `main` or `_start` functions present. Manual tracing through the assembly and decompiled code revealed the following logic:

* **Entry Point (`entry`)**: The program begins at a low-level initialization stub that sets up the environment before jumping to the core malicious payload. This behavior is consistent with **packed/obfuscated malware**.

* **Core Logic**: The core functionality, identified by cross-referencing the suspicious strings, involves reading from a network socket and likely executing commands received from a Command and Control (C2) server. The decompilation shows functions related to network socket creation and I/O.

* **Target Architecture**: Ghidra confirmed the binary is compiled for **ARM**, which suggests its target environment is an **IoT device, router, or other embedded Linux system**.

### 4. Indicators of Compromise (IOCs)

Based on the static analysis, the following IOCs were identified:

| IOC Type | Indicator |
| :--- | :--- |
| **File Hashes** | **SHA-256**: `bcf3f55866d6efe185e0154feecad5808f4e6b7d80898a85e93f667d84e04` |
| | **MD5**: `50f25d01b3b24f38ddb3743198b55a3bf` |
| **Network Artifacts** | HTTP User-Agent string |
| **Persistence Paths** | `/tmp/condi`, `/var/condiBot`, `/var/zxrcr9999` |
| **System Capabilities** | `reboot`, `shutdown`, `wget`, `curl` |

