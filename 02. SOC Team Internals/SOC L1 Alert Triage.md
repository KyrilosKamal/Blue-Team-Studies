## 1. Definition
- **Alert triage** = reviewing, investigating, and classifying SIEM/EDR alerts.
- Goal: separate real threats (True Positive) from noise (False Positive) and benign-but-real activity (Benign True Positive), then route correctly.
- Triage mainly improves **MTTA** (Mean Time to Acknowledge) and **MTTR** (Mean Time to Respond).
- **MTTD** (Mean Time to Detect) measures how fast the threat is detected at all, before any alert is opened. It depends on detection coverage and rule quality, not on triage.

## 2. Why Defenders Care
- A SOC ingests millions of logs, yielding dozens to hundreds of alerts per shift.
- Without a systematic method, critical alerts (active ransomware) get buried under low-severity noise (port scans).
- Alert fatigue makes analysts sloppy: batch-closing, skipping raw logs.
- Attackers can exploit this by generating noise. It is a weakness in SOC process, not a formal ATT&CK technique.

## 3. How It Works
**Simple**
- Event happens (failed login) → SIEM rule matches ("5 failures") → alert ticket created → L1 checks if real attack or forgotten password → close or escalate.

**Technical**
- Telemetry streams into the SIEM.
- Correlation rules (KQL/SPL) evaluate the data.
- Threshold met → alert object created with Severity, Status, Assignee.
- Triage: query SIEM by alert artifacts (IP, hash, user, host), enrich with Threat Intel (VirusTotal, MISP), update status in SIEM/SOAR.

## 4. Key Terms
| Term | Meaning |
|---|---|
| Event | Single observable occurrence (e.g., Event ID 4625) |
| Alert | Notification from one or more events matching a rule |
| True Positive (TP) | Rule fired correctly, activity is malicious |
| False Positive (FP) | Rule fired, activity is benign. Rule likely needs tuning |
| Benign True Positive (BTP) | Rule fired correctly, activity real, but **authorized** (e.g., sanctioned vuln scan by IT) |
| SLA | Max time to triage by severity (e.g., 15 min for Critical) |
| Runbook / Playbook | Step-by-step guide for a specific alert type |
| MTTD | Mean Time to Detect (threat occurs → detected) |
| MTTA | Mean Time to Acknowledge (alert fires → analyst picks it up) |
| MTTR | Mean Time to Respond/Resolve |

Sentinel classification values: `TruePositive`, `FalsePositive`, `BenignPositive`, `Undetermined`.

## 5. Attacker View + MITRE ATT&CK
Noise generation can bury a real attack, but ATT&CK does not model "alert fatigue". Map the actual techniques only.

| Technique | ID | Note |
|---|---|---|
| Network Service Discovery | T1046 | Port/service scanning. Noisy by nature |
| Brute Force | T1110 | Mass failed logons |
| Acquire Infrastructure: Server | T1583.004 | Cheap VPS used for scanning. Blocking the IP does not stop the attacker |
| Indicator Removal | T1070 | Log clearing/deletion. **Not** "hiding in noise" |

## 6. Defender View
**Logs**
- SIEM audit logs (who closed what, when).
- Alert/incident metadata (status, classification, comments).

**SOC QA detection**
- Detect careless closures: High-severity incident closed in seconds with no comment and no playbook run.

**Triage workflow**
1. **Assign & acknowledge**: claim ticket (status → In Progress).
2. **Contextualize**: pull surrounding telemetry.
   - Example: alert for `mimikatz.exe` → check **Event ID 4688** or **Sysmon Event ID 1** for process creation and parent process; use **4624** to see which account logged on to that host.
3. **Verdict**: TP, FP, or BTP.
4. **Action**: escalate to L2 (TP), tune rule (FP), document approval source (BTP).

## 7. Prioritization
Standard order:
1. Filter out assigned / in-progress alerts.
2. Sort by Severity (Critical → Low).
3. Within severity, oldest first.

Real-world adjustments:
- **Asset criticality**: alert on a Domain Controller outranks the same alert on a kiosk.
- **SLA time remaining**: alerts close to breach move up.
- **Alert type/fidelity**: ransomware or credential-dumping indicators outrank generic scans.
- **Correlation**: multiple alerts sharing an IP, user, or host raise priority.

## 8. Mini Scenario (Attack → Detect → Respond)
- **Attack**: attacker runs noisy Nmap against the perimeter FortiGate while exploiting an internal web app via SQL Injection.
- **Detect**: SIEM raises 5,000 "Port Scan" alerts (Low) and 1 "SQL Injection" alert (High).
- **Respond**:
  1. Sort by severity. SQLi alert first.
  2. Check WAF/web logs. Payload succeeded → mark **TP**, escalate to L2.
  3. For the scan alerts: group by source IP. Confirm FortiGate dropped them.
  4. **Correlate**: did any scan source IP also touch the web app or appear in the SQLi alert? If yes, same actor. Add to the incident.
  5. Only then bulk-close the remaining scan noise.
- **Classification of scan alerts**:
  - Scan from an unknown external IP, blocked, no other activity → **TP (blocked, low impact)**, documented. Or FP if the rule is too noisy.
  - **Not BTP.** BTP requires the activity to be authorized (e.g., IT's scheduled scanner).
- Lesson: a distraction scan and a real attack can run together. Batch-closing without correlation is the trap.

## 9. Detection Sample (SOC QA, Sentinel KQL)
**Goal**: find incidents closed too fast with no analyst comment.

```kusto
SecurityIncident
| where Status == "Closed"
| extend TimeToClose = datetime_diff('second', ClosedTime, CreatedTime)
| where TimeToClose < 30
| where isnull(Comments) or array_length(Comments) == 0
| project CreatedTime, Title, Severity, Classification, Owner, TimeToClose
```

Notes:
- `SecurityIncident` tracks incident lifecycle (created/closed times, owner, classification, comments). `SecurityAlert` does not track analyst closure.
- `SecurityIncident` logs a row per incident update. To get the latest state, use `| summarize arg_max(TimeGenerated, *) by IncidentNumber` before filtering.
- Check the `Comments` column format in your workspace schema. Adjust if needed.
- Also consider `TimeToClose` near zero on **High** severity only, to cut noise.

## 10. Free Tools
- **Shuffle SOAR**: automate triage (alert → VirusTotal lookup → auto-close or escalate).
- **TheHive**: case management and alert tracking.
- **MISP**: threat intel sharing, IOC enrichment.

Caution: auto-closing based on a clean VirusTotal score alone is risky. Keep human review on high-severity alerts.

## 11. Labs
- **LetsDefend.io**: simulated SOC dashboard. Practice L1 triage.
- **TryHackMe**: SOC Level 1 path, SOC Simulator, Alert Triage rooms.
- **Blue Team Labs Online (BTLO)**: investigations with PCAP and SIEM logs. Decide TP vs FP.

## 12. Common Mistakes
- **Close-itis**: seeing 50 identical alerts, closing the 50th without checking raw logs.
- **Ignoring SLAs**: 45 minutes on a Medium alert while a Critical sits unassigned.
- **Poor notes**: "Checked, looks fine" is useless.
  - Better: "Source IP checked on VT, clean. User confirmed authorized admin activity. Ticket #1234."
- **Mislabeling BTP**: calling any blocked/expected-looking activity "benign" without proof of authorization.
- **No correlation**: closing noise without checking if the same source did something else.
- **Prioritizing by severity only**: ignoring asset criticality.

## 13. Recap
1. Triage = systematic filtering of alerts into TP, FP, BTP.
2. Triage improves MTTA/MTTR. MTTD is a detection metric.
3. Prioritize: filter assigned → severity → age, adjusted for asset criticality and SLA.
4. BTP = real activity that is **authorized**. Unknown external scans are not BTP.
5. Noise can hide a real attack. Always correlate before bulk-closing.
6. Good notes and SLA discipline matter as much as technical skill.

## 14. Self-Test
1. Alert fires for a known malicious IP, but IT confirms they are running an authorized vuln scan from it. Verdict?
2. How do you order a queue of 100 new alerts?
3. Which MITRE technique covers an attacker scanning your perimeter for open ports and services?
4. Which Windows event/Sysmon ID shows process creation and parent process?
5. Why is batch-closing 5,000 scan alerts as BTP wrong?
6. Which metric does triage speed improve: MTTD or MTTA?

<details>
<summary>Answers</summary>

1. Benign True Positive (BTP). Document the authorization source.
2. Filter out assigned → sort by severity → oldest first. Adjust for asset criticality and SLA.
3. T1046 Network Service Discovery.
4. Windows Security **4688** (process creation) or **Sysmon Event ID 1**.
5. BTP requires authorization. Unknown scans are not authorized. Also risks missing correlation with a real attack.
6. MTTA (and MTTR). MTTD is about detection.
</details>

## 15. Quick Revision Notes
- Event → Alert → Triage → Verdict → Action.
- TP = malicious. FP = benign, tune rule. BTP = real but authorized.
- MTTD ≠ MTTA ≠ MTTR.
- Always correlate by IP/user/host before bulk-closing.
- Process creation: 4688 / Sysmon 1. Logon: 4624.
- T1046 scanning, T1110 brute force, T1070 log clearing.
- Alert fatigue is a process risk, not an ATT&CK technique.
- Notes should let the next analyst continue without restarting.
