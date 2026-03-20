# Cloud IAM Key Exposure

- **Severity baseline:** High
- **Target SLA:** Acknowledge in 10 minutes, containment in 1 hour
- **MITRE ATT&CK (typical):** T1528, T1552, T1098

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
- **Email security stack:** `<tool>`
- **IAM / SSO provider:** `<tool>`
- **Cloud providers:** `<aws|azure|gcp|hybrid>`
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

- Alert or analyst observation indicates this scenario.
- Confidence is medium+ or business impact is plausible.

- **Decision logic (L1):**
  - IF alert source is trusted and confidence >= medium THEN create incident and continue triage.
  - ELSE continue validation workflow (artifact checks, secondary logs, intel lookup) for max 15 minutes.
  - IF confirmed compromise THEN escalate to Tier 2 immediately.
  - IF vendor involvement not confirmed THEN keep incident in validation branch and request vendor telemetry.
- **Severity and confidence gates:**
  - Critical: active compromise + privileged access or regulated data risk.
  - High: credible compromise with lateral movement potential, no confirmed data loss.
  - Medium: suspicious activity with partial evidence, no confirmed impact.
- **Target SLA:** detection to triage <= 15 minutes.
## Immediate actions (0–15 min)

1. Validate alert fidelity and asset criticality.
2. Open incident ticket and assign incident owner.
3. Capture volatile evidence (logs, process tree, account activity, network telemetry).
4. Apply no-regret controls that do not destroy evidence.
5. Notify duty lead if blast radius is unclear.

- **IF / THEN / ELSE flow:**
  - IF identity compromise indicators present THEN force token revocation + password reset path.
  - IF endpoint compromise indicators present THEN isolate host via EDR network containment.
  - ELSE keep host online and collect volatile evidence first.
- **Business impact translation:**
  - Record customer impact status (none / degraded / exposed).
  - Record regulatory trigger status (none / possible / likely) and notify Legal on possible/likely.
- **Automation vs human:**
  - Automate IOC enrichment, WHOIS/reputation lookups, and SIEM pivot queries.
  - Human approval required for production containment affecting customer services.
- **Failure modes:** false-positive isolation, delayed ticket ownership, evidence overwrite; detect via missing timeline updates >10 min.
## Containment

- Isolate affected identities/assets/workloads.
- Block known malicious indicators (domains, hashes, IPs, senders, URLs).
- Force credential reset/token revocation where identity abuse is likely.
- Add temporary detections for related indicators.

- **Decision logic:**
  - IF blast radius includes privileged identities or Tier-0 systems THEN escalate severity to Critical and page Incident Commander.
  - IF compromise remains unconfirmed after containment step THEN maintain reversible controls and continue validation.
- **Time-bound actions:** triage to containment <= 30 minutes; escalation <= 10 minutes after critical trigger.
- **Tool mapping:** SIEM (scope validation), EDR/XDR (host isolation), IAM/SSO (session revocation), CASB/SaaS logs (cloud session blocks), TIP (IOC confidence).
- **Business impact owners to inform:** SOC Lead, Service Owner, CISO; Legal if potential data exposure.
- **Failure modes:** over-containment causing outage, under-containment allowing spread; detect by new related alerts after containment window.
## Investigation and scoping

- Determine patient zero, initial access vector, and timeline.
- Identify all touched systems/accounts/data.
- Confirm whether privileged access or sensitive data was involved.
- Separate confirmed compromise from suspicious-but-unconfirmed noise.

- **Required enrichments:** threat intel campaign correlation, vendor risk profile, actor TTP alignment, known exploit-chain check.
- **Blast radius estimation checklist:**
  - Affected systems count and business criticality tier.
  - Privileged account exposure confirmed/suspected.
  - Lateral movement indicators (remote admin, token misuse, anomalous east-west traffic).
- **Decision logic:**
  - IF privileged access confirmed THEN Critical severity and Tier 2/IR takeover.
  - IF only isolated single asset and no privilege escalation THEN remain High/Medium per confidence.
- **Automation vs human:**
  - Automate timeline stitching and entity correlation.
  - Human validates root cause and compromise narrative.
## Eradication and recovery

- Remove persistence, malicious artefacts, and unauthorised access paths.
- Patch/harden control gaps exploited in the incident.
- Recover systems safely and monitor for re-entry.
- Run post-recovery validation checks.

- **Deterministic flow:**
  - IF persistence mechanism identified THEN remove + verify via second-source telemetry.
  - IF patch/control not available THEN apply compensating controls and risk acceptance approval.
  - ELSE patch, rotate secrets, and re-baseline detections.
- **SLA target:** containment to recovery plan <= 4 hours (Critical) / <= 8 hours (High).
- **Business impact:** require service owner sign-off before restoring externally exposed workloads.
- **Failure modes:** incomplete eradication, reinfection, stale credentials; detect via 24h enhanced monitoring and regression checks.
## Comms and escalation

- Internal: SOC lead, IT ops, identity, legal/compliance as required.
- External: regulator/customers/partners only through approved channels.
- Use concise updates: what happened, impact, actions, next checkpoint.

- **Who to inform and when:**
  - Immediate: SOC Lead + Incident Commander on confirmed compromise.
  - <=30 min: CISO / Security leadership for High/Critical incidents.
  - Legal/Privacy immediately on potential regulated data impact.
  - Vendor security contacts when third-party dependency is implicated.
- **Escalation logic:** IF evidence quality drops or scope expands rapidly THEN escalate one severity level and increase update cadence.
- **Update cadence:** Critical every 30 min, High every 60 min, Medium every 120 min.
## Evidence and documentation

- Preserve raw logs, timeline, IoCs, and decision points.
- Document every containment action with timestamp and actor.
- Attach forensic artefacts and command outputs to the incident record.

- **Standardised outputs (mandatory):**
  - Incident summary: `<what happened> <who affected> <impact> <current status> <next action>`.
  - Timeline format: `UTC timestamp | actor/system | action | evidence link | analyst`.
  - Evidence checklist: alerts, raw logs, host artefacts, network telemetry, IAM events, IOC list, containment commands, approvals.
- **Automation:** auto-export SIEM query results and hash artefacts to evidence store.
- **Failure modes:** broken chain-of-custody, missing timestamps; detect via case QA before closure.
## Exit criteria

- Threat activity stopped and validated.
- Affected scope fully inventoried and addressed.
- Recovery complete, owners signed off.
- Lessons learned captured with dated action items.

- **Decision closure gates:**
  - IF any unresolved high-risk indicator remains THEN incident cannot close.
  - IF vendor confirmation pending for key indicator THEN keep case in monitoring state, not closed.
- **Post-incident improvement loop (required):**
  - Reassess vendor trust/risk tier and contract notification requirements.
  - Document control gaps and assign owners + due dates.
  - Add/adjust detections, SOAR play steps, and suppression rules.
  - Update this playbook with lessons learned within 5 business days.
## RACI (default)

| Function | Responsibility |
| --- | --- |
| SOC Analyst (L1/L2) | **Responsible** for triage, evidence capture, and ticket hygiene |
| Incident Commander / SOC Lead | **Accountable** for decision-making and escalation |
| IT / Platform / Cloud Ops | **Responsible** for containment and recovery actions |
| Legal / Compliance / Privacy | **Consulted** when notification or regulatory impact is possible |
| Business Owner / Service Owner | **Informed** on impact, ETA, and closure status |
## Communication templates

### Internal update (High)
`Incident update: <scenario> under investigation. Impact: <impact>. Scope confidence: <low/med/high>. Current actions: <actions>. Next update in 60 minutes.`

### Stakeholder update (High)
`A security event is being actively handled for <service/system>. At present, impact is <impact status>. We will provide the next update by <time>.`
## War room mode (one-page checklist)

- [ ] Incident owner assigned
- [ ] Timeline started (UTC)
- [ ] Affected assets/accounts list created
- [ ] Containment actions approved and executed
- [ ] Detection coverage expanded for related IoCs
- [ ] Exec/stakeholder update sent at agreed interval
- [ ] Recovery validation completed
- [ ] Post-incident actions captured with owners and due dates
