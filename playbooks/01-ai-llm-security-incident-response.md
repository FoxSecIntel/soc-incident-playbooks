# AI LLM Security Incident Response: Indirect Prompt Injection, Model Poisoning, Sensitive Data Leakage

- **Severity baseline:** High
- **Target SLA:** Acknowledge in 10 minutes, containment in 30 minutes
- **MITRE ATT&CK (typical):** ATLAS AML.T0051 Prompt Injection, AML.T0018 Data Poisoning, AML.T0044 Exfiltration via Model, AML.T0034 Lateral Movement via AI Supply Chain
- **OWASP Top 10 for LLM Applications (2025/2026):** LLM01 Prompt Injection, LLM02 Insecure Output Handling, LLM06 Sensitive Information Disclosure, LLM08 Excessive Agency, LLM09 Overreliance
- **IR alignment model:** **Hybrid-Adaptive**: NIST 800-61 lifecycle + MITRE ATLAS adversary behaviours + OWASP LLM control weaknesses

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
- **AI Platform / MLOps owner:** `<name> <contact>`
- **Data Engineering / Vector DB owner:** `<name> <contact>`
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
- **LLM gateway / guardrails:** `<tool>`
- **Prompt observability / tracing:** `<tool>`
- **Vector database / RAG store:** `<tool>`
- **Model registry and hash store:** `<tool>`
- **Secret manager for model/API keys:** `<tool>`

## Pre-flight incident variables

- **Incident ID:** `<id>`
- **Incident severity:** `<sev>`
- **Detection source / rule:** `<rule-name-or-id>`
- **Initial detection time (UTC):** `<timestamp>`
- **Known affected user(s):** `<users>`
- **Known affected host(s):** `<hosts>`
- **Known IoCs:** `<ioc-list>`
- **Affected model(s):** `<model-name/version/hash>`
- **Affected prompt/system policy version:** `<policy-version>`
- **Affected embedding index / collection:** `<index-name>`

## Trigger conditions

- LLM output shows hidden intent shifts, policy bypasses, unusual tool calls, or sensitive data leakage.
- RAG answers include suspicious external instructions, malicious hyperlinks, or unexpected privileged actions.
- Model behaviour changes after data refresh, fine-tune, or dependency update.

- **Decision logic (L1):**
  - IF **confirmed compromise** in model behaviour or leaked sensitive data THEN escalate to Tier 2 immediately.
  - ELSE IF vendor or upstream component compromise is not confirmed THEN continue validation workflow for up to 15 minutes.
  - IF confidence remains low after 15 minutes THEN escalate as High due to uncertain AI blast radius.
- **Severity and confidence gates:**
  - **Critical:** confirmed sensitive data disclosure, privileged workflow abuse, or malicious model/tool execution at scale.
  - **High:** credible prompt injection or poisoning indicators with production exposure but no confirmed regulated data leak.
  - **Medium:** semantic anomaly with constrained scope, no policy bypass proven.
  - **Low:** benign anomaly, test artefact, or false positive.
- **Target SLA:** detection to triage less than 15 minutes.

## Immediate actions (0–15 min)

1. Validate incident context: model version, deployment ring, prompt template version, and RAG index lineage.
2. Freeze evidence: prompt/response traces, tool-call logs, model routing logs, API gateway logs, and guardrail outcomes.
3. Activate AI incident bridge and assign Incident Commander.
4. Apply no-regret protections that preserve service continuity where possible.
5. Notify SOC lead and AI platform owner.

- **IF / THEN / ELSE flow:**
  - IF output indicates sensitive data leakage THEN block response rendering path and mask stored transcripts.
  - IF indirect prompt injection is suspected THEN quarantine affected retrieval sources and disable risky tool connectors.
  - ELSE continue controlled observation with strict monitoring for 15 minutes.
- **Barbell Strategy containment start:**
  - **Automation side:** rate limiting, enhanced logging, suspicious prompt tagging, temporary response filtering.
  - **HITL side:** model rollback approval, system prompt changes, weight isolation, production policy overrides.
- **Business impact translation:**
  - Data exposure risk: map leaked fields to data classification.
  - Customer impact: identify affected channels, sessions, and geographies.
  - Regulatory implications: identify GDPR/sector notification triggers.
- **Who to inform:** SOC, CISO delegate for High/Critical, Legal/Privacy if regulated data risk exists.

## Containment

- Isolate AI blast radius without full platform shutdown.
- Define and enforce **AI Air Gap** controls:
  - Isolate affected **Vector Database** collections from retrieval pipeline.
  - Rotate and scope-restrict LLM and tool **API keys**.
  - Disable high-risk tool calls while keeping read-only low-risk paths available.
  - Route traffic to safe fallback model/profile where available.

- **Decision logic:**
  - IF active exploitation continues after first controls THEN escalate to Critical and isolate affected model endpoint.
  - IF customer-facing output remains unsafe THEN force safe response mode with human review queue.
  - ELSE maintain degraded but safe service with enhanced monitoring.
- **Time-bound actions:** triage to containment less than 30 minutes, escalation less than 10 minutes after Critical trigger.
- **Tool mapping:** SIEM correlation, LLM gateway policy switches, CASB/SaaS logs, IAM token revocation, TIP enrichment.
- **Failure modes:** over-isolation causing outage, incomplete AI Air Gap allowing continued retrieval poisoning.
- **Early failure detection:** continued policy-violation responses after containment, new malicious tool-call patterns.

## Investigation and scoping

- Determine attack class: indirect prompt injection, poisoning, leakage, or hybrid chain.
- Focus on semantic and behavioural indicators, not just static IoCs.
- Correlate with threat intelligence and known campaign patterns targeting AI supply chains.

- **Required enrichments:**
  - Prompt lineage and hidden instruction markers.
  - Retrieval document provenance and ingestion timestamps.
  - Vendor risk profile and recent advisories.
  - Privileged action audit for model tools and agents.
- **Blast radius estimation checklist:**
  - Affected models, endpoints, tenants, and user cohorts.
  - Privileged tool invocation attempts and success rate.
  - Lateral movement risk into CI/CD, model registry, or secret stores.
- **Decision logic:**
  - IF poisoning source is confirmed in a specific corpus THEN isolate that corpus and all derivative embeddings.
  - IF model hash mismatch is confirmed THEN treat as potential model tampering and escalate to Critical.
  - ELSE continue scoped validation and maintain enhanced controls.

## Eradication and recovery

- Purge malicious artefacts and restore trusted AI state.
- **Poisoning eradication workflow:**
  - Remove poisoned source documents from ingestion pipeline.
  - Rebuild embeddings for affected collections only.
  - Verify clean retrieval outputs against test suites.
- **Model integrity workflow:**
  - Compare running model artefact hash with registry **Known-Good hash**.
  - Roll back to **Known-Good model hash** when mismatch or unsafe drift is confirmed.
  - Re-sign and re-attest model package before redeploy.

- **Decision logic:**
  - IF rollback candidate fails safety tests THEN hold deployment, switch to fallback service profile.
  - ELSE redeploy controlled canary and monitor semantic safety metrics.
- **SLA target:** containment to recovery plan less than 4 hours for Critical, less than 8 hours for High.
- **Human decision required:** any model rollback, system prompt rewrite, or policy exception.
- **Failure modes:** incomplete embedding purge, re-poisoning through unchanged ingestion jobs.

## Comms and escalation

- Internal: SOC, AI platform owner, MLOps, Data Engineering, Security leadership.
- External: customers, regulators, vendors only through approved legal/comms path.
- Maintain concise updates with impact, controls, confidence, and next checkpoint.

- **Who to inform and when:**
  - Immediate: SOC lead, Incident Commander, AI platform owner.
  - Within 30 minutes for High/Critical: CISO and business service owner.
  - Immediate on regulated data risk: Legal/Privacy.
  - Vendor contact when upstream model, plugin, or dependency is implicated.
- **Update cadence:** Critical every 30 minutes, High every 60 minutes, Medium every 120 minutes.

## Evidence and documentation

- Preserve complete prompt-response telemetry with immutable timestamps.
- Preserve model artefact metadata: hashes, signatures, deployment manifests.
- Preserve RAG ingestion and retrieval evidence: source IDs, embedding job IDs, index snapshots.

- **Standardised outputs (mandatory):**
  - **Incident summary template:** `<what happened> <AI component affected> <data risk> <customer impact> <current controls> <next action>`.
  - **Timeline format:** `UTC timestamp | component | event | evidence URI | analyst`.
  - **Evidence checklist:** traces, prompts, responses, tool calls, model hashes, embedding lineage, key rotation logs, approvals.
- **Automation:** automatic export of trace bundles and integrity metadata to evidence store.
- **Failure modes:** missing provenance, inconsistent time sources, overwritten logs.

## Exit criteria

- Exploitation path blocked and verified by red-team style replay tests.
- Affected models and embeddings restored to trusted state.
- Privileged tool access and key scopes revalidated.
- Business owner, AI owner, and security owner sign-off completed.

- **Decision closure gates:**
  - IF any unresolved leakage risk remains THEN incident cannot close.
  - IF model integrity attestation is incomplete THEN keep incident open.
- **Post-incident improvement loop (required):**
  - Update retrieval filtering and prompt hardening controls.
  - Reassess vendor and dependency risk tier.
  - Add new detections for semantic anomalies and hidden intent markers.
  - Run tabletop and red-team scenarios for the same kill chain.

## Model drift analysis (post-mortem)

- Compare pre-incident and post-fix behaviour across safety, accuracy, and hallucination metrics.
- Validate that remediation did not increase refusal errors or unsafe hallucinations.
- Execute benchmark suite on critical intents and high-risk policy prompts.
- Record drift deltas and approve release only when risk is lower than pre-incident baseline.

## Comparison: AI-specific playbook vs legacy IT playbook

| Area | AI-specific playbook (this) | Legacy IT response playbook |
| --- | --- | --- |
| Detection focus | Semantic anomalies, hidden intent, tool-call behaviour | Static IoCs, signature-based indicators |
| Containment model | AI Air Gap for vector DB, keys, retrieval paths | Host/network isolation only |
| Integrity checks | Model hash attestation, embedding lineage validation | File integrity and endpoint artefacts |
| Eradication | Purge poisoned embeddings, rollback to Known-Good model hash | Malware removal, patching, credential reset |
| Risk governance | HITL approvals for model rollback and prompt policy edits | Standard change approvals, often not AI-aware |
| Recovery validation | Safety and hallucination drift analysis | Service restoration and availability checks |

## RACI (default)

| Function | Responsibility |
| --- | --- |
| SOC Analyst (L1/L2) | **Responsible** for triage, evidence capture, and ticket hygiene |
| Incident Commander / SOC Lead | **Accountable** for decision-making and escalation |
| AI Platform / MLOps | **Responsible** for model, gateway, and deployment controls |
| Data Engineering / RAG Owner | **Responsible** for embedding and retrieval remediation |
| Legal / Compliance / Privacy | **Consulted** when notification or regulatory impact is possible |
| Business Owner / Service Owner | **Informed** on impact, ETA, and closure status |

## Communication templates

### Internal update (Critical)
`AI incident update: <scenario> active. Affected components: <model/index/gateway>. Data risk: <status>. Customer impact: <status>. Containment: <status>. Next update in 30 minutes.`

### Stakeholder update (Critical)
`We are responding to an AI security incident affecting <service>. Current impact: <impact>. Immediate containment controls are active, and we are validating model and retrieval integrity. Next update by <time>.`

## War room mode (one-page checklist)

- [ ] Incident owner assigned
- [ ] Timeline started (UTC)
- [ ] Affected models, indexes, and keys listed
- [ ] AI Air Gap controls applied
- [ ] Tier 2 escalated on confirmed compromise
- [ ] Evidence bundle exported with model and embedding lineage
- [ ] Recovery canary and drift analysis completed
- [ ] Playbook and detections updated with lessons learned
