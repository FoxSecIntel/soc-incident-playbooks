# SOC Incident Playbooks

High-signal, operator-ready incident response playbooks for day-to-day SOC operations.

## Why this repo

Most playbooks are either too vague or too long. This set is designed for **live incidents**:
- clear first 15-minute actions
- containment and eradication checklists
- escalation and communications prompts
- evidence handling and closure standards

## How to use

1. Pick the matching scenario below.
2. Follow **Immediate Actions** first.
3. Work top-to-bottom through containment, investigation, and recovery.
4. Complete closure actions and lessons learned.

## Playbook Index (15)

1. [AI LLM Security Incident Response: Indirect Prompt Injection, Model Poisoning, Sensitive Data Leakage](playbooks/01-ai-llm-security-incident-response.md)
2. [Autonomous Agent Hijack: Indirect Prompt Injection with Tool-Use Abuse](playbooks/02-autonomous-agent-hijack.md)
3. [Phishing Email Triage and Response](playbooks/03-phishing-email-triage.md)
4. [Business Email Compromise (BEC)](playbooks/04-business-email-compromise.md)
5. [Ransomware Incident Response](playbooks/05-ransomware-response.md)
6. [Endpoint Malware Detection and Containment](playbooks/06-endpoint-malware-response.md)
7. [Impossible Travel / Suspicious Sign-in](playbooks/07-impossible-travel-signin.md)
8. [Brute-force / Credential Stuffing](playbooks/08-bruteforce-credential-stuffing.md)
9. [Privileged Account Misuse](playbooks/09-privileged-account-misuse.md)
10. [Data Exfiltration Suspected](playbooks/10-data-exfiltration.md)
11. [Cloud IAM Key Exposure](playbooks/11-cloud-iam-key-exposure.md)
12. [Suspicious PowerShell / LOLBin Activity](playbooks/12-powershell-lolbin-activity.md)
13. [Web Application Attack (WAF/Logs)](playbooks/13-web-app-attack-response.md)
14. [DDoS Service Degradation](playbooks/14-ddos-service-degradation.md)
15. [Third-party / Supply-chain Compromise](playbooks/15-third-party-supply-chain-compromise.md)


## Design standard used in every playbook

- **Priority and SLA** (what must happen now)
- **Immediate actions (0–15 min)**
- **Containment**
- **Investigation and scoping**
- **Eradication and recovery**
- **Comms and escalation**
- **Evidence and documentation**
- **Exit criteria and lessons learned**

## Suggested next improvements

- Map each playbook to your SIEM detections and automation rules.
- Add environment-specific contact trees and escalation numbers.
- Add one-page “war room mode” checklists per playbook.


## v2 operational upgrades

- Added **RACI** matrix to each playbook.
- Added **severity-aware communication templates** (Medium/High/Critical).
- Added **war room one-page checklist** to each playbook.
- Added **SIEM query stubs** (Sentinel KQL, Splunk SPL, Elastic KQL): see [`siem-query-stubs/`](siem-query-stubs/).

## SIEM stub index

- [01 ai llm security incident response](siem-query-stubs/01-ai-llm-security-incident-response-siem-stubs.md)
- [02 autonomous agent hijack](siem-query-stubs/02-autonomous-agent-hijack-siem-stubs.md)
- [03 phishing email triage](siem-query-stubs/03-phishing-email-triage-siem-stubs.md)
- [04 business email compromise](siem-query-stubs/04-business-email-compromise-siem-stubs.md)
- [05 ransomware response](siem-query-stubs/05-ransomware-response-siem-stubs.md)
- [06 endpoint malware response](siem-query-stubs/06-endpoint-malware-response-siem-stubs.md)
- [07 impossible travel signin](siem-query-stubs/07-impossible-travel-signin-siem-stubs.md)
- [08 bruteforce credential stuffing](siem-query-stubs/08-bruteforce-credential-stuffing-siem-stubs.md)
- [09 privileged account misuse](siem-query-stubs/09-privileged-account-misuse-siem-stubs.md)
- [10 data exfiltration](siem-query-stubs/10-data-exfiltration-siem-stubs.md)
- [11 cloud iam key exposure](siem-query-stubs/11-cloud-iam-key-exposure-siem-stubs.md)
- [12 powershell lolbin activity](siem-query-stubs/12-powershell-lolbin-activity-siem-stubs.md)
- [13 web app attack response](siem-query-stubs/13-web-app-attack-response-siem-stubs.md)
- [14 ddos service degradation](siem-query-stubs/14-ddos-service-degradation-siem-stubs.md)
- [15 third party supply chain compromise](siem-query-stubs/15-third-party-supply-chain-compromise-siem-stubs.md)


## v3 SOC god-tier upgrades

- Added **environment profile** section to every playbook (tenant, critical assets, service scope).
- Added **on-call and escalation directory** fields to every playbook.
- Added **tooling map** fields (SIEM, EDR, IAM, ticketing, evidence store).
- Added **pre-flight incident variables** section for rapid war-room setup.
- Added reusable templates under [`templates/`](templates/):
  - [Environment profile template](templates/environment-profile-template.md)
  - [Severity/SLA matrix](templates/severity-sla-matrix.md)
  - [War room roles template](templates/war-room-roles-template.md)

### Fast rollout path

1. Complete `templates/environment-profile-template.md` once.
2. Copy values into each playbook’s environment section.
3. Confirm SLA matrix with leadership and legal.
4. Run one tabletop exercise per high/critical playbook.
