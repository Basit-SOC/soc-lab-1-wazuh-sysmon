## Attack Simulation

The attack was carried out from the Kali Linux machine (192.168.56.101) against the 
Windows 10 victim machine (192.168.56.1). An Nmap service and OS detection scan 
(`nmap -sV -A`) was run to enumerate open ports, running services, and the target 
operating system.

The scan identified several open ports, including SMB (445), NetBIOS (139), and RPC 
(135), and correctly fingerprinted the host as Windows 10. It also flagged that SMB 
message signing was enabled but not required — a configuration weakness that could 
be leveraged in relay-style attacks.


<img width="1600" height="1181" alt="image" src="https://github.com/user-attachments/assets/9796705f-06cd-4342-94a3-697369b1591a" />


*Figure 1: Nmap service/OS detection scan run from Kali against the Windows 10 victim*

---

## Detection Overview

Following the reconnaissance and follow-on activity, Wazuh flagged multiple alerts 
tied to discovery behavior. The top alerts observed were dominated by **discovery 
activity** and **net.exe account discovery**, alongside expected baseline activity 
like Windows logon success and service startup events.

At the rule-group level, most alerts came from **windows** and **sysmon** rule sets, 
confirming that Sysmon telemetry was the primary source feeding Wazuh's detection 
logic. The dashboard also mapped several alerts to **PCI DSS requirements** (10.2.5, 
10.2.6, 10.6, 10.6.1), which relate to logging and monitoring of user activity — 
useful context if this lab is ever framed around compliance-driven monitoring.

<img width="1600" height="377" alt="image" src="https://github.com/user-attachments/assets/27a75859-837e-499a-8dfb-4bcc92d04a56" />



*Figure 2: Wazuh dashboard breakdown — top alerts, rule groups, and PCI DSS mapping*

This maps to MITRE ATT&CK **T1087 — Account Discovery** and **T1059 — Command and 
Scripting Interpreter**, since net.exe and PowerShell were the mechanisms used to 
enumerate accounts.

---

## Alert Volume Summary

Over a 24-hour window, Wazuh logged **118 total alerts** on the monitored endpoint, 
with **1 alert at Level 12 or above** — indicating one higher-severity event stood 
out from routine noise. No authentication failures were recorded, and 20 successful 
authentications occurred, suggesting the activity did not involve brute-force login 
attempts but rather post-access discovery behavior.

The alert timeline shows a clear spike in activity concentrated in a short window, 
consistent with the attack simulation being run in a single, deliberate session 
rather than activity spread out over time.

<img width="1600" height="377" alt="image" src="https://github.com/user-attachments/assets/07104571-4b3f-42ea-ae1f-24bbd69d0ef6" />

*Figure 3: Wazuh Security Events overview — alert totals and activity timeline*

---

## Impact Assessment

If this activity occurred outside a controlled lab, the reconnaissance and discovery 
behavior observed would allow an attacker to:
- Map the target's exposed services and OS version to plan further exploitation
- Enumerate local accounts to identify targets for lateral movement or privilege escalation
- Operate with low noise, since no failed logins were generated to trip simpler 
  brute-force detections

Severity was rated **Medium** because the activity was contained to an isolated lab 
host with no evidence of lateral movement or data exfiltration.

---

## Remediation / Response

In a real environment, the following steps would be taken:
1. Investigate the source host running the Nmap scan and net.exe discovery commands.
2. Review Sysmon Event ID 1 (process creation) logs around the alert spike to 
   identify the exact commands executed.
3. Restrict SMB signing to "required" rather than "enabled but not required" to 
   reduce relay-attack risk.
4. Correlate the discovery activity with any account or group changes in the same 
   time window to rule out follow-on persistence attempts.

---

## Lessons Learned

This lab confirmed that Wazuh + Sysmon can reliably detect:
- Network reconnaissance activity indirectly through follow-on host behavior
- Discovery commands executed via net.exe and PowerShell
- Alert volume spikes tied to a single attack session, useful for timeline reconstruction

---

## Conclusion

The simulated attack chain — reconnaissance via Nmap followed by discovery activity 
on the victim host — was detected and visualized end-to-end through the Wazuh 
dashboard. The lab demonstrates a working detection pipeline from raw Sysmon 
telemetry through to alert triage at the dashboard level.
