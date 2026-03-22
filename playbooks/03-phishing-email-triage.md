# Phishing Email Triage and Response
- **Last reviewed:** 2026-03-22

- **Severity baseline:** Medium
- **Target SLA:** Acknowledge in 15 minutes, containment in 30 minutes
- **MITRE ATT&CK (typical):** T1566, T1204, T1114, T1078
- **OWASP alignment (adjacent):** Social engineering and identity abuse pathways

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
- **Email security stack / SEG:** `<tool>`
- **IAM / SSO provider:** `<tool>`
- **Cloud providers:** `<aws|azure|gcp|hybrid>`
- **Ticketing / case system:** `<tool>`
- **Forensics evidence store:** `<location>`
- **Threat intel platform (TIP):** `<tool>`
- **Mailbox search and purge tooling:** `<tool>`

## Pre-flight incident variables

- **Incident ID:** `<id>`
- **Incident severity:** `<sev>`
- **Detection source / rule:** `<rule-name-or-id>`
- **Initial detection time (UTC):** `<timestamp>`
- **Known affected user(s):** `<users>`
- **Known affected host(s):** `<hosts>`
- **Known IoCs:** `<ioc-list>`

## Trigger conditions

- User report, SEG alert, or SOC analytics indicate phishing or social engineering.
- Confidence is medium+, or business impact is plausible.
- Message may have no malicious attachment/link but requests sensitive or financial action.

- **Decision logic (L1):**
  - IF alert source is trusted and confidence >= medium THEN create incident and continue triage.
  - ELSE continue validation workflow for max 15 minutes.
  - IF confirmed compromise THEN escalate to Tier 2 immediately.
  - IF vendor involvement not confirmed THEN continue validation workflow.
- **Severity and confidence gates:**
  - **Critical:** confirmed credential theft, payment diversion, privileged account impact, or broad campaign spread.
  - **High:** strong malicious indicators with user interaction, no confirmed takeover yet.
  - **Medium:** suspicious campaign pattern with constrained impact.
  - **Low:** false positive with no campaign indicators.
- **Target SLA:** detection to triage <= 15 minutes.

## Immediate actions (0–15 min)

1. Validate alert fidelity and asset criticality.
2. Open incident ticket and assign incident owner.
3. Preserve raw message evidence: headers, body, attachments, URLs, Message-ID.
4. Trigger campaign scoping search across tenant mailboxes.
5. Notify duty lead if blast radius is unclear.

- **Low-risk actions that should be automated immediately:**
  - URL/domain reputation checks using VirusTotal, URLScan, passive DNS and TIP.
  - **New-Born Domain** checks:
    - IF domain age < 30 days THEN increase risk score one tier.
    - IF domain age < 7 days and financial/credential request exists THEN classify High pending further proof.
  - Tenant-wide similarity search:
    - sender/reply-to patterns
    - subject/body fingerprint
    - URL path and tracking token reuse
  - Auto-quarantine known matching messages across all mailboxes.
- **High-risk actions requiring human decision:**
  - VIP sender verification via out-of-band identity confirmation.
  - Determining AI-generated social engineering where language is flawless but intent is malicious.
  - Root cause analysis of SEG/gateway miss before permanent policy changes.
- **Business impact translation:**
  - Data exposure risk: classify affected information.
  - Customer impact: identify exposed user population.
  - Regulatory implications: determine if legal notification thresholds may be met.

## Containment

- Isolate affected identities, mail artefacts, and malicious infrastructure indicators.
- Execute broad-spectrum tenant-wide containment.

- **Broad containment controls:**
  - Purge matching and near-matching messages from all mailboxes.
  - Block sender domains, reply-to domains, malicious URLs, and attachment hashes.
  - Revoke tokens and reset credentials for users who clicked/submitted credentials.
  - Disable suspicious OAuth applications and mailbox forwarding rules.
- **Decision logic:**
  - IF blast radius includes privileged users or finance/payment workflows THEN escalate severity to Critical.
  - IF compromise remains unconfirmed THEN use reversible controls and continue validation.
  - IF campaign remains active after first purge THEN tighten SEG policy and repeat sweep.
- **Time-bound actions:** triage to containment <= 30 minutes, escalation <= 10 minutes after critical trigger.
- **Tool mapping:** SIEM, SEG, EDR/XDR, IAM/SSO, TIP, SaaS audit logs.
- **Who to inform:** SOC lead, service owner, CISO; Legal/Privacy if potential regulated data risk.
- **Failure modes:** over-containment causing business disruption, under-containment allowing continued spread.

## Investigation and scoping

- Determine patient zero, campaign origin, and full blast radius.
- Prioritise campaign analysis over single-message handling.

- **Campaign scoping requirements:**
  - Identify all recipients, clickers, repliers, and credential submitters.
  - Identify linked infrastructure: domains, redirectors, cloud-hosted pages.
  - Identify temporal clusters and variant templates.
- **AI-augmented phishing analysis:**
  - Perform **semantic anomaly** review:
    - unusual urgency or authority pressure
    - process bypass requests
    - polished language inconsistent with sender history
  - Analyse **tone** for internal impersonation and business-context mimicry.
  - Treat no-link/no-attachment payment or credential requests as high suspicion.
- **Decision logic:**
  - IF privileged access confirmed THEN Critical severity and Tier 2/IR takeover.
  - IF only isolated user with no interaction THEN maintain Medium with monitoring.
- **Automation vs human:**
  - Automate timeline stitching, recipient mapping, IOC correlation.
  - Human validates intent and impersonation plausibility.

## Eradication and recovery

- Remove persistence and residual access created by phishing workflow.
- Restore normal operations with strengthened controls.

- **Deterministic flow:**
  - IF mailbox rule abuse identified THEN remove rules and re-audit mailbox delegates.
  - IF OAuth abuse identified THEN revoke grants and enforce consent controls.
  - IF credentials exposed THEN reset passwords, revoke sessions, enforce phishing-resistant MFA.
- **Recovery checks:**
  - Validate no further message variants are being delivered.
  - Validate affected users have clean sign-in activity.
  - Validate payment and supplier-change workflows are restored with additional approvals.
- **SLA target:** containment to recovery plan <= 4 hours for Critical, <= 8 hours for High.
- **Failure modes:** hidden forwarding rules, delayed user reporting, token persistence.

## Comms and escalation

- Internal: SOC lead, IT ops, identity, legal/compliance as required.
- External: regulator/customers/partners only through approved channels.
- Use concise updates: what happened, impact, actions, next checkpoint.

- **Who to inform and when:**
  - Immediate: SOC lead and Incident Commander on confirmed compromise.
  - <=30 minutes: CISO/security leadership for High/Critical.
  - Immediate: Legal/Privacy when regulated data risk is possible.
  - Finance leadership for payment-diversion campaigns.
- **Escalation logic:** IF scope expands rapidly or evidence quality degrades THEN escalate one severity level.
- **Update cadence:** Critical every 30 minutes, High every 60 minutes, Medium every 120 minutes.

## Evidence and documentation

- Preserve raw logs, timeline, IoCs, and decision points.
- Document every containment action with timestamp and actor.
- Attach forensic artefacts and command outputs to the incident record.

- **Standardised outputs (mandatory):**
  - Incident summary: `<what happened> <who affected> <impact> <current status> <next action>`.
  - Timeline format: `UTC timestamp | actor/system | action | evidence link | analyst`.
  - Evidence checklist:
    - headers and raw MIME
    - URL/domain enrichment
    - mailbox search/purge results
    - click/credential telemetry
    - token revocation and reset logs
    - approvals and communications logs
- **Automation:** auto-export SIEM and SEG query artefacts to evidence store.
- **Failure modes:** chain-of-custody gaps, missing timestamps, partial tenant search.

## Exit criteria

- Threat activity stopped and validated.
- Affected scope fully inventoried and addressed.
- Recovery complete, owners signed off.
- Lessons learned captured with dated action items.

- **Decision closure gates:**
  - IF unresolved high-risk indicators remain THEN incident cannot close.
  - IF campaign origin and spread are not fully scoped THEN incident cannot close.
- **Post-incident improvement loop (required):**
  - Assess why SEG or AI filters failed to intercept message variants.
  - Update campaign detections and similarity-matching rules.
  - Improve VIP verification and payment-change controls.
  - Update this playbook and SOAR workflow within 5 business days.

## RACI (default)

| Function | Responsibility |
| --- | --- |
| SOC Analyst (L1/L2) | **Responsible** for triage, evidence capture, and ticket hygiene |
| Incident Commander / SOC Lead | **Accountable** for decision-making and escalation |
| IT / Platform / Cloud Ops | **Responsible** for containment and recovery actions |
| Legal / Compliance / Privacy | **Consulted** when notification or regulatory impact is possible |
| Business Owner / Service Owner | **Informed** on impact, ETA, and closure status |

## Communication templates

### Internal update (Critical)
`Incident update: phishing campaign active. Current impact: <impact>. Scope: <scope>. Containment: <status>. Next update in 30 minutes.`

### Stakeholder update (Critical)
`We are responding to a phishing campaign affecting <service/group>. Current customer impact: <impact/none confirmed>. Immediate controls are in place and investigation is ongoing. Next update by <time>.`

## War room mode (one-page checklist)

- [ ] Incident owner assigned
- [ ] Timeline started (UTC)
- [ ] Blast radius search completed across tenant mailboxes
- [ ] Tenant-wide quarantine and block actions executed
- [ ] Privileged and finance users verified and protected
- [ ] Exec/stakeholder update sent at agreed interval
- [ ] Recovery validation completed
- [ ] Detection and playbook updates captured with owners and due dates
