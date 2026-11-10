# Blue Team Studies

Personal notes, labs, and detection content from my defensive security (Blue Team) learning path. Built with AI-assisted study notes, then verified against official docs and hands-on labs.

## Goals

- Learn SOC, detection engineering, DFIR, threat hunting
- Understand attacker techniques to build better detections (MITRE ATT&CK)
- Build a public portfolio of notes, queries, and lab write-ups

## Structure

```
Blue-Team-Studies/
├── notes/          # Topic notes (one file per topic)
├── detections/     # Sigma rules, SPL/KQL queries
├── labs/           # Lab write-ups (TryHackMe, LetsDefend, BTLO)
├── cheatsheets/    # Quick references (Event IDs, commands)
└── resources/      # Books, courses, links
```

## Note Format

Each topic follows the same template:

1. Definition
2. Why defenders care
3. How it works
4. Key terms
5. Attacker view (MITRE ATT&CK IDs)
6. Defender view (logs, Event IDs, detection, response)
7. Mini scenario (attack → detect → respond)
8. Detection sample (Sigma / SPL / KQL)
9. Tools
10. Lab idea
11. Common mistakes
12. Recap
13. Self-test questions

## Topics Covered

| Topic | Status |
|-------|--------|
| Windows Event Logs | ☐ |
| Sysmon | ☐ |
| SIEM basics | ☐ |
| Kerberos attacks & detection | ☐ |
| Phishing analysis | ☐ |
| Memory forensics | ☐ |
| Network traffic analysis | ☐ |

*(Update as notes are added.)*

## Tools & Platforms

Wazuh · Splunk Free · ELK · Velociraptor · Zeek · Suricata · Sysmon · Wireshark · Volatility

Labs: TryHackMe · LetsDefend · Blue Team Labs Online · CyberDefenders

## Disclaimer

Notes are for learning and may contain mistakes. Verify Event IDs, ATT&CK IDs, and detection logic before production use. No offensive tooling or exploit code in this repo. Lab content only on authorized platforms.

## Contributing

Found an error? Open an issue or PR.

## Author

**Kyrillos Kamal**
[GitHub](https://github.com/KyrilosKamal) · [LinkedIn](https://linkedin.com/in/kyrillos-kamal)
