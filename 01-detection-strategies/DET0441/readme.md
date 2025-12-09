# CAR-2013-09-003 – SMB Session Setups

## Overview

**CAR ID:** CAR-2013-09-003  
**Analytic Name:** SMB Session Setups  
**Category:** Network-based analytic  
**Primary Focus:** Monitoring SMB session setup activity to detect potential lateral movement or reconnaissance.

This folder documents my end-to-end implementation of the analytic:
- Lab setup used
- Attack simulation steps
- Telemetry captured (pcaps, logs)
- Detection logic in multiple languages
- Findings, limitations, and improvements

---

## MITRE References

- **MITRE CAR page:** https://car.mitre.org/analytics/CAR-2013-09-003/
- **Mapped ATT&CK Techniques:**  
  - https://attack.mitre.org/techniques/T1187/
  - Technique: Forced Authentication
  - Tactic: Credential Access

---

## Files in This Folder

- `lab-steps.md` – Exact steps to reproduce the lab & attack.
- `analysis.md` – Telemetry review, explanation, and findings.
- `notes.md` – Scratchpad, false positives, tuning notes.
- `detections/` – Detection rules (Sigma, KQL, SPL, EQL, Zeek).
- `evidence/` – Screenshots, pcaps, log samples.

---

## Status

- 🔄 Lab built  
- 🔄 Attack simulated  
- 🔄 Telemetry captured  
- 🔄 First version of detections written  
- 🔄 Tuning & improvements ongoing  

---

## High-Level Summary

*(Fill this in once you’re done)*

- **Goal:** Capture user password hash for brute-forcing offline
- **Approach:** Generate SMB connections between Windows hosts using Kali/Windows client, capture traffic with Zeek/Wireshark, and build detection logic to identify abnormal fan-out / patterns.
- **Outcome:**  
  - Detection reliably flags scanning/lateral movement patterns.  
  - Normal SMB usage from clients to file servers generates baseline logs.  
  - Identified key fields useful for enrichment (username, hostname, logon status).
