# Static Malware Analysis Summary

This repository contains the deliverables for **Task 7: Static Malware Analysis**. The objective of this task was to analyze a suspicious file without executing it, documenting its functionality and indicators of compromise.

## 📋 Analysis Steps

The analysis was conducted in a safe, isolated environment and followed a three-phase methodology:

1.  **Triage & Hashing**: The file was identified, and its cryptographic hashes were calculated.

2.  **Static Inspection**: The binary was inspected using command-line tools (`strings`) and a reverse engineering tool (Ghidra) to extract embedded data and understand its logical flow.

3.  **Reporting**: All findings were compiled into a comprehensive report.

## 📁 Deliverables

All analysis results, screenshots, and raw data are included in this repository. The main deliverables are as follows:

* `report.md`: A detailed report containing the full analysis, findings, and conclusion.

* `suspicious_strings.txt`: The raw output of the `strings` command.

* `screenshots/`: A directory containing all visual evidence from the analysis process.

Please refer to `report.md` for the complete analysis of the sample.
