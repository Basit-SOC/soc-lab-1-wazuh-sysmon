# Incident Report — Lab 1 Home SOC Lab

## Date: July 8, 2026
## Analyst: Basit Ali
## Severity: Medium
## Status: Resolved

## Environment
- SIEM: Wazuh 4.7.5 on Ubuntu 192.168.56.30
- Victim: Windows 10 Pro 192.168.56.1 with Sysmon and Wazuh Agent
- Attacker: Kali Linux 192.168.56.101

## Attack 1 — Reconnaissance Port Scan
- Tool: Nmap
- Command: nmap -sV -A 192.168.56.1
- Findings: Open ports 135 139 445 7070 discovered. OS identified as Windows 10.
- Wazuh detection: Discovery activity alert generated

## Attack 2 — Persistence Backdoor Account
- Command: net user hacker Password123 /add
- Wazuh detection: Rule 60109 User account created Level 8
- MITRE technique: T1098 Account Manipulation
- Action taken: Fake account deleted

## Conclusion
Both attacks were successfully detected by Wazuh. Backdoor account creation generated highest severity alerts at Level 8.

## Lessons Learned
- Wazuh detects account manipulation reliably
- Sysmon provides detailed process and command logging
- Network attacks require Suricata IDS for better detection covered in Lab 3
