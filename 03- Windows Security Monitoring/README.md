# Windows Security Monitoring

> **Status:** Planned — practical work has not started.

## Objectives

Build the ability to collect, interpret, and investigate Windows endpoint telemetry.

## Learning sequence

| Step | Topic | Practical evidence |
|---|---|---|
| 1 | Windows Event Viewer and Security log | Export and explain selected events |
| 2 | Logon events: 4624 and 4625 | Correlate account, source, time, and Logon Type |
| 3 | Process creation: 4688 | Identify parent/child process relationships |
| 4 | Sysmon installation and configuration | Confirm which event types are actually collected |
| 5 | PowerShell logging | Inspect available script-block and module telemetry |
| 6 | Investigation workflow | Build a timeline from multiple event sources |

## Important validation notes

- Event IDs alone are not proof of malicious activity; interpret event fields and surrounding context.
- Event 4624 represents a successful logon and 4625 a failed logon. Logon Type and account context matter.
- Event 4688 process command-line visibility depends on audit policy/configuration.
- Sysmon Event ID 3 (Network connection) is configuration-dependent and may not be collected.
- Record Windows version, audit policy, Sysmon version, and configuration used in each lab.

## First practical case

**Scenario:** Repeated failed logons followed by a successful logon.

Questions to answer:

1. Are the failures associated with the same account and source?
2. What Logon Type is recorded?
3. Is the successful logon from the same source?
4. What happened immediately before and after the successful logon?
5. What additional evidence is required before calling this a compromise?

Use [the Lab Report Template](../00-%20Templates/Lab%20Report%20Template.md) for the write-up.

## References

- [Windows Security Auditing](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/basic-audit-logon-events)
- [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Windows Event Log](https://learn.microsoft.com/en-us/windows/win32/wes/windows-event-log)
