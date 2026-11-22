# Blue Team Studies

A structured learning journal for SOC operations, Windows security monitoring, detection engineering, and incident investigation.

The repository separates **conceptual notes** from **hands-on lab evidence**. Notes may be AI-assisted; commands, queries, Event IDs, and detection logic must be validated against the referenced platform, official documentation, or a reproducible lab before being treated as confirmed.

## Learning objectives

- Understand SOC workflows, alert triage, escalation, and incident reporting.
- Interpret Windows and network telemetry.
- Build and validate detections using Sigma and SIEM query languages.
- Investigate activity using evidence and map findings to MITRE ATT&CK.
- Maintain reproducible lab write-ups with commands, outputs, and conclusions.

## Repository structure

```text
Blue-Team-Studies/
├── 00- Templates/
│   ├── Lab Report Template.md
│   └── Note Template.md
├── 01- Blue Team Introduction/
│   ├── 01. Junior Security Analyst Intro.md
│   ├── 02. SOC Role in Blue Team.md
│   ├── 03. Humans as Attack Vectors.md
│   └── 04. Systems as Attack Vectors.md
├── 02. SOC Team Internals/
│   ├── 01. SOC L1 Alert Triage.md
│   ├── 02. SOC L1 Alert Reporting.md
│   └── 03. SOC Workbooks and Lookups.md
├── 03- Windows Security Monitoring/
│   └── README.md
└── 04- Detection Engineering/
    └── README.md
```

## Current progress

| Area | Status | Evidence |
|---|---|---|
| Blue Team fundamentals | Notes completed | Four topic notes |
| SOC team operations | Notes completed | Three topic notes |
| Windows security monitoring | Planned | Lab not started |
| Detection engineering | Planned | Rules and queries not validated |
| Incident investigation | Planned | Practical case studies pending |

**Status definitions**

- **Notes completed:** a written study note exists; this does not imply practical mastery.
- **Lab completed:** the procedure was executed and the report includes environment, steps, relevant output, and analysis.
- **Detection validated:** the rule/query was tested against relevant telemetry, including expected matches and false-positive considerations.

## Note and lab standards

Use the templates in [00- Templates](00-%20Templates/) when adding material.

A useful practical write-up should include:

1. Objective and scope
2. Lab environment and versions
3. Exact steps or commands
4. Relevant raw output / screenshots (redact secrets and personal data)
5. Interpretation: what the evidence proves and what it does not prove
6. Detection or investigation logic
7. MITRE ATT&CK mapping, where applicable
8. Limitations, false positives, and follow-up actions
9. References

Do not label a query or detection as tested unless it was actually executed. Do not publish credentials, tokens, private IP details from a real environment, or sensitive organizational data.

## Planned technical areas

- Windows Security Event Log and logon types
- Sysmon telemetry and process creation
- PowerShell logging
- Wazuh / Splunk ingestion and investigation
- Network traffic analysis with Wireshark
- Sigma rules and SIEM queries
- Alert triage, threat hunting, and incident reporting

## References

- [MITRE ATT&CK](https://attack.mitre.org/)
- [Microsoft Learn](https://learn.microsoft.com/)
- [Sysmon documentation](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Wazuh documentation](https://documentation.wazuh.com/)
- [Sigma documentation](https://sigmahq.io/)

## Disclaimer

This is a personal learning repository, not production detection guidance. Validate technical details and test detection logic in an authorized environment before operational use. All lab activity must be limited to systems you own or are explicitly authorized to assess.

## Author

**Kyrillos Kamal**  
[GitHub](https://github.com/KyrilosKamal) · [LinkedIn](https://linkedin.com/in/kyrillos-kamal)
