# SOC Lab 1 — Home SIEM Lab

## Overview
A home SOC lab built to practice real-world security monitoring and incident response skills.

## Lab Architecture
- Wazuh SIEM Server — Ubuntu 22.04 — 192.168.56.30
- Windows 10 Victim Machine — Sysmon + Wazuh Agent — 192.168.56.1
- Kali Linux Attacker Machine — 192.168.56.101

## Tools Used
- Wazuh 4.7.5 — SIEM and alert engine
- Sysmon — Windows telemetry and logging
- Nmap — Port scanning and reconnaissance
- Kali Linux — Attack simulation

## Attacks Simulated
1. Nmap port scan — reconnaissance
2. Backdoor account creation — persistence

## Detections
- Rule 60109 — User account created Level 8
- Rule 60110 — User account changed Level 8
- Rule 92033 — Discovery via PowerShell
- Rule 92039 — net.exe account discovery

## MITRE ATT&CK Techniques Covered
- T1087 — Account Discovery
- T1098 — Account Manipulation
- T1484 — Group Policy Modification
- T1059 — Command and Scripting Interpreter

## Files
- incident-report-lab1.md — Full incident report

## Screenshots
Coming soon
