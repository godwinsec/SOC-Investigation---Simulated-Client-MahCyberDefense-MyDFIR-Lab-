#  Incident Report  
**Suspicious PowerShell Execution and Failed Lateral Movement Attempt**

---

## 1. Date of Report : **2025-12-08**


## 2. Reported By : **SOC-106 (Godwin Ifeanyi Uche)**


## 3. Severity Level
- **Medium** – Malicious Execution  
- **High** – Attempted Domain Controller Access

---

##  4. Executive Summary

On **2025-12-03**, a Base64-encoded PowerShell command executed on **mts-contractorpc2** which dropped and ran **TMulQVsNluoRhcJVp.exe** with a secondary payload. Registry modifications indicated attempted persistence. Shortly after execution, the **administrator** account performed multiple failed logon attempts against the **domain controller (mts-dc)**, indicating attempted lateral movement but no successful compromise.

All activity occurred in a **simulated SOC environment** under MahCyberDefense / MyDFIR.

---

## Tools Used

| Tool | Purpose |
|------|----------|
| Microsoft Defender XDR | Alert triage, incident linkage, case management |
| Microsoft Advanced Hunting (KQL) | Process, registry, and logon event investigation |
| PowerShell Base64 Decoder (CyberChef) | Decoding malicious encoded commands |
| Splunk (Simulated) | Cross-log correlation and alert forwarding (not primary in this case) |
| MITRE ATT&CK | Technique classification |
| Windows Registry Analysis (XDR telemetry) | Persistence validation |


---

##  5. Summary of Findings

- PowerShell dropper executed on **mts-contractorpc2**
- AutoIt-based payload deployed
- Registry modification consistent with persistence
- 200+ failed DC authentication attempts using administrator credentials
- No propagation or successful lateral access detected

---

##  6. Investigation Timeline (UTC)

| Time | Event |
|------|-------|
| 03:41 | PowerShell executed Base64 payload |
| 03:41 | AutoIt dropper **TMulQVsNluoRhcJVp.exe** launched |
| 03:41 | Registry persistence entry created |
| 03:58 | >200 failed administrator logon attempts to **mts-dc** |

---

## 7. Who / What / When / Where / Why / How

| Category | Details |
|----------|---------|
| **Who** | Administrator account on `mts-contractorpc2` |
| **What** | Base64 PowerShell dropper & attempted DC access |
| **When** | 2025-12-03, between 03:41–03:58 UTC |
| **Where** | Source: `mts-contractorpc2` → Target: `mts-dc` |
| **Why** | Likely privilege escalation & persistence attempt |
| **How** | Encoded PowerShell → AutoIt dropper → failed DC logon attempts |

---

## 8. MITRE ATT&CK Mapping

| Technique | Code |
|-----------|------|
| PowerShell Execution | **T1059.001** |
| Persistence via Registry | **T1547** |
| Lateral Movement Attempt | **T1021** |
| Credential Access / Brute Force | **T1110** |
| Remote Process Execution | **T1059** |

---

##  9. Impact Assessment

- Attempted lateral movement toward **Domain Controller**
- No successful authentication
- No spread to secondary assets
- Activity contained to single simulated host

---

##  10. Recommendations

1. Isolate **mts-contractorpc2**
2. Terminate malicious processes:
   - `powershell.exe`
   - `TMulQVsNluoRhcJVp.exe`
3. Remove malicious autorun registry keys
4. Delete payloads from **LOCALAPPDATA**
5. Reset administrator credentials & revoke active sessions
6. Full endpoint malware scan & cleanup
7. Continue monitoring failed DC logons for recurrence

---


##  11. Status
**Open** – Containment documented, pending SOC lead review

---

## 12. Reviewed By
(To be completed by **Steven Mah / SOC Supervisor**)

---

### Environment Disclosure

This report was generated as part of the **MahCyberDefense / MyDFIR simulated SOC training program**.  
All clients, devices, and incidents described are **simulated environments used for educational purposes**.

> *Note: All data originates from simulated training systems.*

