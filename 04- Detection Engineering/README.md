# Detection Engineering

> **Status:** Planned — no detection in this section is currently claimed as validated.

## Detection lifecycle

1. Define the behavior and threat hypothesis.
2. Identify the required telemetry and confirm it is being collected.
3. Write a detection query or Sigma rule.
4. Test against representative positive and benign data.
5. Review missed cases and false positives.
6. Document tuning, limitations, and response guidance.

## Detection record

| Field | Description |
|---|---|
| Detection name | Short, descriptive title |
| Objective | Behavior the detection should identify |
| Data source | Windows Security, Sysmon, DNS, proxy, etc. |
| Required fields | Fields needed by the logic |
| ATT&CK mapping | Technique ID when justified |
| Logic | Sigma / SPL / KQL |
| Positive test | Evidence that should match |
| Negative test | Benign evidence that should not match |
| Limitations | Visibility gaps and known blind spots |
| Status | Draft / Tested / Tuned |

## Validation rules

- Do not assume a field exists in every SIEM or event schema.
- Confirm collection and parsing before troubleshooting query logic.
- Distinguish a matching event from a confirmed incident.
- A blocked action can still be a valid detection of attempted behavior; record prevention separately from detection.
- Include benign test cases and explain expected false positives.

## References

- [Sigma specification](https://sigmahq.io/)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [Wazuh documentation](https://documentation.wazuh.com/)
- [Splunk Search Reference](https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/WhatsInThisManual)
