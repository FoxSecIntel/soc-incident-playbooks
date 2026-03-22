# SOC-04: Malicious Browser Extension
- **Last reviewed:** 2026-03-22

- **Severity baseline:** High
- **Target SLA:** Acknowledge in 10 minutes, containment in 30 minutes
- **MITRE ATT&CK (typical):** T1176, T1539, T1550, T1056
- **Primary 2026 risk:** Session cookie theft to bypass MFA, semantic data scraping from AI chat interfaces, unauthorised OAuth token use

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
- **Endpoint / EDR owner:** `<name> <contact>`
- **Browser management owner:** `<name> <contact>`
- **Identity / IAM owner:** `<name> <contact>`
- **Legal / Privacy:** `<name> <contact>`
- **Executive escalation:** `<name> <contact>`

## Tooling map (your environment)

- **SIEM:** `<sentinel|splunk|elastic|other>`
- **EDR/XDR:** `<tool>`
- **Browser fleet management:** `<intune|jamf|gpo|other>`
- **CASB / SaaS logs:** `<tool>`
- **IAM / SSO provider:** `<tool>`
- **Threat intel platform:** `<tool>`
- **Forensics evidence store:** `<location>`

## Pre-flight incident variables

- **Incident ID:** `<id>`
- **Incident severity:** `<sev>`
- **Detection source / rule:** `<rule-name-or-id>`
- **Initial detection time (UTC):** `<timestamp>`
- **Extension name:** `<name>`
- **Extension ID:** `<id>`
- **Publisher / developer certificate:** `<publisher>`
- **Known affected users:** `<users>`
- **Known affected endpoints:** `<hosts>`

## Trigger conditions

- Browser extension with suspicious permissions appears in managed fleet.
- User sessions show unusual token reuse or cookie replay patterns.
- Data leaves AI portals to unknown domains after user interaction.

- **Decision logic (L1):**
  - IF extension is unapproved and has high-risk permissions (`cookies`, `webRequest`, `tabs`, `storage`) THEN escalate to High immediately.
  - IF confirmed session hijack or privileged account impact THEN escalate to Critical and page Incident Commander.
  - IF compromise is plausible but unconfirmed THEN continue validation for max 15 minutes.

## Immediate actions (0–15 min)

1. Open incident ticket and assign owner.
2. Capture extension metadata: **ID**, version, install source, permissions, hash.
3. Freeze evidence: browser logs, extension files, network telemetry, token activity.
4. Start tenant-wide hunt for same extension ID and publisher.

- **Automated low-risk actions:**
  - Enrich extension ID and publisher via threat intel feeds.
  - Correlate extension install events across all endpoints.
  - Flag users with recent AI portal access plus suspicious extension network egress.

- **Human-required actions:**
  - Validate business legitimacy of extension with endpoint and app owners.
  - Confirm whether observed behaviour is policy-violating or malicious.

## Containment

- Use a dual-track containment model for speed and control.

- **Automation track:**
  - Immediate fleet-wide uninstallation by **Extension ID**.
  - Push block policy for reinstallation of the same ID.
  - Blacklist the **developer certificate / publisher fingerprint** where supported.
  - Block known command-and-control and exfiltration domains.

- **Human-in-the-loop track:**
  - Forensic review of suspected **remote staging** behaviour.
  - Manual revocation of all active OAuth and session tokens for impacted users.
  - Manual review before broad domain blocks that might disrupt legitimate SaaS.

- **SLA targets:**
  - Detection to triage: `< 15 minutes`
  - Triage to containment: `< 30 minutes`
  - Critical escalation: `< 10 minutes after confirmation`

## Investigation and scoping

- Determine campaign scope across users, devices, browsers, and SaaS platforms.
- Prioritise endpoints with privileged users and AI tooling access.

- **Detection focus:**
  - **Permission Creep:** extension updates that add high-risk permissions without approval.
  - **Semantic Egress:** outbound traffic to unknown domains immediately after user interactions in AI portals.

- **Analysis checks:**
  - Compare first-seen and latest extension permission sets.
  - Map extension activity to account token events and suspicious sign-ins.
  - Identify all affected systems and high-value datasets accessed.

## Manifest and code analysis

- Inspect extension package and runtime configuration.

- **manifest.json review:**
  - `permissions`, `host_permissions`, `optional_permissions`
  - `content_scripts` targeting AI domains or wildcard patterns
  - `background` / `background_page` logic and persistence patterns
  - `externally_connectable`, `web_accessible_resources`, update URL

- **Behavioural analysis:**
  - Detect dynamic code fetch and remote script staging.
  - Detect DOM scraping of chat content, prompts, responses, and file uploads.
  - Detect cookie/session token extraction paths.

## Eradication and recovery

- Remove extension threat and reset browser trust state.

- **Eradication steps:**
  - Remove malicious extension from all profiles and devices.
  - Purge cached extension artefacts and local storage residues.
  - Rotate affected credentials and invalidate browser sessions.

- **Mandatory browser profile sanitisation:**
  - Clear profile-level extension data, service worker caches, and storage artefacts.
  - Re-apply managed browser baseline policies.
  - Validate approved extension allow-list synchronisation.

- **Mandatory Cookie Reset:**
  - Force sign-out across all business SaaS sessions.
  - Revoke session cookies and refresh tokens.
  - Enforce step-up authentication on first post-incident sign-in.

## Comms and escalation

- Internal: SOC, endpoint team, IAM, legal/privacy as required.
- External: regulators/customers/vendors through approved comms channels only.

- **Who to inform and when:**
  - Immediate: SOC lead, endpoint owner, IAM owner.
  - Within 30 minutes for High/Critical: CISO delegate and service owners.
  - Immediate: Legal/Privacy when sensitive data exposure is likely.

## Evidence and documentation

- Preserve extension package, hashes, install/update timeline, and policy decisions.
- Preserve browser telemetry, token events, and network flow logs.

- **Standard outputs:**
  - Incident summary: `<what happened> <extension ID> <affected users> <impact> <controls> <next action>`
  - Timeline format: `UTC | component | event | owner | evidence URI`
  - Evidence checklist: manifest, permission diffs, network captures, token revocation logs, endpoint actions

## Exit criteria

- Malicious extension removed and blocked tenant-wide.
- All impacted sessions and OAuth tokens revoked.
- Browser profiles sanitised and policy baseline restored.
- Monitoring confirms no reinfection or reinstallation.

## Comparison: 2026 Browser-Specific vs Generic Malware Playbook

| Area | 2026 Browser-Specific Playbook | Generic Malware Playbook |
| --- | --- | --- |
| Initial risk model | Session hijack and SaaS token abuse | Host compromise only |
| Detection focus | Permission creep, semantic egress, extension telemetry | Process and file indicators |
| Containment method | Fleet extension uninstall + certificate block + token revocation | Endpoint isolation and AV clean-up |
| Recovery requirement | Browser profile sanitisation and mandatory cookie reset | Host rebuild or malware removal |
| AI data protection | Explicit checks for AI portal scraping and prompt leakage | Usually absent |

## RACI (default)

| Function | Responsibility |
| --- | --- |
| SOC Analyst (L1/L2) | **Responsible** for triage and evidence capture |
| Incident Commander / SOC Lead | **Accountable** for escalation and decisions |
| Endpoint / Browser Team | **Responsible** for uninstall and policy enforcement |
| IAM Team | **Responsible** for token and session revocation |
| Legal / Compliance / Privacy | **Consulted** for regulatory impact |
| Business Owner / Service Owner | **Informed** on impact and recovery timeline |

## Communication templates

### Internal update (Critical)
`Incident update: malicious browser extension confirmed. Extension ID: <id>. Affected users/endpoints: <scope>. Containment status: <status>. Next update in 30 minutes.`

### Stakeholder update (Critical)
`We are responding to a browser-extension security incident affecting user sessions and SaaS access. Containment controls are active, and impacted sessions are being reset. Next update by <time>.`

## War room mode (one-page checklist)

- [ ] Incident owner assigned
- [ ] Extension ID blocked and uninstalled fleet-wide
- [ ] Developer certificate blacklisted
- [ ] Impacted OAuth/session tokens revoked manually
- [ ] Browser profile sanitisation complete
- [ ] Cookie reset complete for impacted users
- [ ] Evidence bundle exported
- [ ] Detection and policy improvements logged
