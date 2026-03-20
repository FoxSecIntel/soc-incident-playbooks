# Phishing Email Triage and Response

- **Severity baseline:** Medium
- **Target SLA:** Acknowledge in 15 minutes, containment in 1 hour
- **MITRE ATT&CK (typical):** T1566, T1204

## Trigger conditions

- Alert or analyst observation indicates this scenario.
- Confidence is medium+ or business impact is plausible.

## Immediate actions (0–15 min)

1. Validate alert fidelity and asset criticality.
2. Open incident ticket and assign incident owner.
3. Capture volatile evidence (logs, process tree, account activity, network telemetry).
4. Apply no-regret controls that do not destroy evidence.
5. Notify duty lead if blast radius is unclear.

## Containment

- Isolate affected identities/assets/workloads.
- Block known malicious indicators (domains, hashes, IPs, senders, URLs).
- Force credential reset/token revocation where identity abuse is likely.
- Add temporary detections for related indicators.

## Investigation and scoping

- Determine patient zero, initial access vector, and timeline.
- Identify all touched systems/accounts/data.
- Confirm whether privileged access or sensitive data was involved.
- Separate confirmed compromise from suspicious-but-unconfirmed noise.

## Eradication and recovery

- Remove persistence, malicious artefacts, and unauthorised access paths.
- Patch/harden control gaps exploited in the incident.
- Recover systems safely and monitor for re-entry.
- Run post-recovery validation checks.

## Comms and escalation

- Internal: SOC lead, IT ops, identity, legal/compliance as required.
- External: regulator/customers/partners only through approved channels.
- Use concise updates: what happened, impact, actions, next checkpoint.

## Evidence and documentation

- Preserve raw logs, timeline, IoCs, and decision points.
- Document every containment action with timestamp and actor.
- Attach forensic artefacts and command outputs to the incident record.

## Exit criteria

- Threat activity stopped and validated.
- Affected scope fully inventoried and addressed.
- Recovery complete, owners signed off.
- Lessons learned captured with dated action items.

## RACI (default)

| Function | Responsibility |
| --- | --- |
| SOC Analyst (L1/L2) | **Responsible** for triage, evidence capture, and ticket hygiene |
| Incident Commander / SOC Lead | **Accountable** for decision-making and escalation |
| IT / Platform / Cloud Ops | **Responsible** for containment and recovery actions |
| Legal / Compliance / Privacy | **Consulted** when notification or regulatory impact is possible |
| Business Owner / Service Owner | **Informed** on impact, ETA, and closure status |

## Communication templates

### Internal update (Medium)
`Security event update: <scenario> triage in progress. Current assessment: <status>. Actions underway: <actions>. Next update in 2 hours or sooner on escalation.`

### Stakeholder update (Medium)
`A security event has been detected and is being assessed. No confirmed major impact at this time. We will share updates if risk level changes.`

## War room mode (one-page checklist)

- [ ] Incident owner assigned
- [ ] Timeline started (UTC)
- [ ] Affected assets/accounts list created
- [ ] Containment actions approved and executed
- [ ] Detection coverage expanded for related IoCs
- [ ] Exec/stakeholder update sent at agreed interval
- [ ] Recovery validation completed
- [ ] Post-incident actions captured with owners and due dates

