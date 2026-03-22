# Autonomous Agent Hijack: Indirect Prompt Injection with Tool-Use Abuse
- **Last reviewed:** 2026-03-22

- **Severity baseline:** Critical
- **Target SLA:** Acknowledge in 5 minutes, containment in 15 minutes
- **MITRE ATLAS (typical):** Prompt Injection, Agent Tool Misuse, Data Exfiltration via Agent Actions
- **IR alignment model:** NIST 800-61 lifecycle + MITRE ATLAS + OWASP LLM controls

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
- **AI Platform / Agent owner:** `<name> <contact>`
- **Identity / IAM owner:** `<name> <contact>`
- **Legal / Privacy:** `<name> <contact>`
- **Executive escalation:** `<name> <contact>`

## Tooling map (your environment)

- **SIEM:** `<sentinel|splunk|elastic|other>`
- **EDR/XDR:** `<tool>`
- **IAM / SSO provider:** `<tool>`
- **Secrets manager:** `<tool>`
- **Agent orchestration platform:** `<tool>`
- **Audit logs for tools:** `GitHub, Jira, financial APIs, cloud APIs`
- **Forensics evidence store:** `<location>`

## Pre-flight incident variables

- **Incident ID:** `<id>`
- **Incident severity:** `<sev>`
- **Detection source / rule:** `<rule-name-or-id>`
- **Initial detection time (UTC):** `<timestamp>`
- **Agent ID / name:** `<agent-id>`
- **Agent run/session ID:** `<run-id>`
- **Known abused tools:** `<tool-list>`
- **Known IoCs / prompts:** `<ioc-list>`

## Trigger conditions

- Agent initiated unauthorised actions in connected systems.
- Prompt or retrieved content indicates hidden instructions.
- Tool-call sequence is inconsistent with approved task scope.

- **Decision logic (L1):**
  - IF unauthorised tool action is confirmed THEN escalate to Tier 2 immediately.
  - IF compromise is plausible but unconfirmed THEN continue validation for max 10 minutes.
  - IF financial API or production code write actions occurred THEN classify **Critical** immediately.

## Immediate actions (0–15 min)

1. Open incident and assign Incident Commander.
2. Freeze logs: agent prompts, tool-call traces, auth logs, API audit logs.
3. Trigger emergency controls.

- **Automated low-risk controls (must run immediately):**
  - Revoke or disable active **agent API tokens** using a central kill switch.
  - Suspend scheduled and recurring agent jobs.
  - Set agent runtime to read-only safe mode where supported.
  - Snapshot current permissions and tool bindings.

- **Human-in-the-loop controls (mandatory):**
  - Review reasoning and trace logs where policy permits.
  - Confirm likely injection source: user prompt, retrieved document, connector payload, memory context.
  - Approve high-impact recovery actions such as policy rollback or production unfreeze.

## Containment

- Isolate the agent blast radius without full platform shutdown.

- **Containment actions:**
  - Disable agent tool execution path.
  - Block outbound calls to high-risk connectors.
  - Quarantine suspect retrieval sources and memory entries.
  - Enforce temporary allow-list for approved prompts and tool targets.

- **SLA targets:**
  - Detection to triage: `< 15 minutes`
  - Triage to containment: `< 30 minutes`
  - Critical escalation: `< 10 minutes after confirmation`

## Investigation and scoping

- Determine attack class: indirect prompt injection, context poisoning, or hybrid chain.
- Correlate with threat intelligence and known campaign patterns.

- **Scoping checklist:**
  - Affected agent runs, users, projects, repos, tickets, transactions.
  - Privileged action attempts and success rate.
  - Lateral movement risk into CI/CD, secrets, or cloud control plane.

## Tool-Use revocation

- Identify exactly which permissions were abused and revoke least privilege first.

- **Revocation workflow:**
  - List all connected tools and scopes for the affected agent.
  - Map each executed action to permission scope.
  - Revoke compromised scopes in this order:
    1. **Write** and **admin** scopes
    2. Financial transfer and approval scopes
    3. Repository mutation scopes
    4. Ticket workflow transition scopes
  - Rotate credentials and issue new scoped tokens with expiry.

- **Decision logic:**
  - IF scope is unknown THEN revoke all write scopes immediately.
  - ELSE revoke confirmed abused scopes plus adjacent high-risk scopes.

## Eradication and recovery

- Remove injection source and restore trusted configuration.

- **Eradication steps:**
  - Purge malicious prompt artefacts from memory and context stores.
  - Remove poisoned retrieval documents and rebuild affected embeddings.
  - Validate policy hierarchy and guardrail precedence.
  - Re-issue agent credentials with minimal scopes.

- **Recovery steps:**
  - Re-enable tools in phased order: read-only, limited-write, approved scopes.
  - Run canary tasks and verify output and action integrity.
  - Require dual approval before restoring financial or production-write actions.

## Comms and escalation

- **Inform immediately:** SOC lead, AI platform owner, IAM owner.
- **Inform within 30 minutes for High/Critical:** CISO delegate, service owner.
- **Inform Legal/Privacy:** if regulated data or customer impact is possible.
- **Inform Finance leadership:** if payment workflows were touched.

## Evidence and documentation

- Preserve:
  - Prompt and response traces
  - Tool-call logs with timestamps and principals
  - Token issuance/revocation logs
  - Affected system audit logs
  - Decision approvals and containment actions

- **Standard output templates:**
  - Incident summary: `<what happened> <agent> <abused tools> <impact> <current controls> <next action>`
  - Timeline: `UTC | component | event | action | owner | evidence URI`
  - Evidence checklist: prompts, traces, API logs, IAM logs, token lifecycle, revocation proof

## Exit criteria

- Unauthorised actions have stopped and are verified.
- Abused permissions are revoked and re-issued with least privilege.
- Injection source is removed and validated.
- Monitoring confirms no recurrence in agreed observation window.
- Owners sign off on recovery.

## RACI (default)

| Function | Responsibility |
| --- | --- |
| SOC Analyst (L1/L2) | **Responsible** for triage and evidence capture |
| Incident Commander / SOC Lead | **Accountable** for escalation and decisions |
| AI Platform / Agent Owner | **Responsible** for agent isolation and policy correction |
| IAM Owner | **Responsible** for token revocation and re-issuance |
| Legal / Compliance / Privacy | **Consulted** for regulatory impact |
| Business Owner / Service Owner | **Informed** on impact and recovery timeline |

## Communication templates

### Internal update (Critical)
`Incident update: autonomous agent hijack confirmed. Impacted agent: <agent-id>. Abused tools: <tools>. Containment status: <status>. Next update in 30 minutes.`

### Stakeholder update (Critical)
`We are responding to a security incident involving an autonomous agent with tool permissions. Immediate containment controls are active and we are validating affected systems and data. Next update by <time>.`

## War room mode (one-page checklist)

- [ ] Incident owner assigned
- [ ] Agent kill switch executed
- [ ] Tool permissions mapped and revoked
- [ ] Injection source identified and quarantined
- [ ] Tier 2 engaged for confirmed compromise
- [ ] Evidence bundle exported
- [ ] Recovery canary validated
- [ ] Detection and playbook updates logged
