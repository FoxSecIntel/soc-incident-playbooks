# SOC-XX: <Playbook title>

- **Severity baseline:** <Critical|High|Medium|Low>
- **Last reviewed:** 2026-03-22
- **Target SLA:** Acknowledge in <x> minutes, containment in <y> minutes
- **MITRE ATT&CK (typical):** <techniques>

## Environment profile (complete before live use)

- **Organisation / BU:** `<org-or-bu>`
- **Primary tenant / account / subscription:** `<tenant-id-or-account-id>`
- **Critical asset group(s):** `<tier0-assets>`
- **Business-critical services:** `<service-names>`
- **Data classes in scope:** `<pii|pci|phi|ip|internal>`
- **Time zone for incident clock:** `<tz>`
- **Incident bridge / war room link:** `<bridge-url>`

## On-call and escalation directory

- **L1/L2 SOC on-call:** `<name> <contact>`
- **Incident Commander:** `<name> <contact>`
- **Identity / IAM owner:** `<name> <contact>`
- **Cloud / Platform owner:** `<name> <contact>`
- **Network / Edge owner:** `<name> <contact>`
- **Endpoint / EDR owner:** `<name> <contact>`
- **Legal / Privacy:** `<name> <contact>`
- **Comms / PR:** `<name> <contact>`
- **Executive escalation:** `<name> <contact>`

## Tooling map (your environment)

- **SIEM:** `<sentinel|splunk|elastic|other>`
- **EDR/XDR:** `<tool>`
- **IAM / SSO provider:** `<tool>`
- **Ticketing / case system:** `<tool>`
- **Forensics evidence store:** `<location>`

## Pre-flight incident variables

- **Incident ID:** `<id>`
- **Incident severity:** `<sev>`
- **Detection source / rule:** `<rule-name-or-id>`
- **Initial detection time (UTC):** `<timestamp>`
- **Known affected user(s):** `<users>`
- **Known affected host(s):** `<hosts>`
- **Known IoCs:** `<ioc-list>`

## Trigger conditions

- <condition>

## Immediate actions (0–15 min)

1. <step>

## Containment

- <step>

## Investigation and scoping

- <step>

## Eradication and recovery

- <step>

## Comms and escalation

- <step>

## Evidence and documentation

- <step>

## Exit criteria

- <step>

## RACI (default)

| Function | Responsibility |
| --- | --- |
| SOC Analyst (L1/L2) | **Responsible** for triage and evidence capture |
| Incident Commander / SOC Lead | **Accountable** for decisions and escalation |
| Platform/Service Owner | **Responsible** for containment and recovery actions |
| Legal / Compliance / Privacy | **Consulted** when notification risk exists |
| Business Owner / Service Owner | **Informed** on impact and ETA |
