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

## Playbook Index (14)

1. [AI LLM Security Incident Response](playbooks/01-ai-llm-security-incident-response.md)
2. [Phishing Email Triage and Response](playbooks/02-phishing-email-triage.md)
3. [Business Email Compromise (BEC)](playbooks/03-business-email-compromise.md)
4. [Ransomware Incident Response](playbooks/04-ransomware-response.md)
5. [Endpoint Malware Detection and Containment](playbooks/05-endpoint-malware-response.md)
6. [Impossible Travel / Suspicious Sign-in](playbooks/06-impossible-travel-signin.md)
7. [Brute-force / Credential Stuffing](playbooks/07-bruteforce-credential-stuffing.md)
8. [Privileged Account Misuse](playbooks/08-privileged-account-misuse.md)
9. [Data Exfiltration Suspected](playbooks/09-data-exfiltration.md)
10. [Cloud IAM Key Exposure](playbooks/10-cloud-iam-key-exposure.md)
11. [Suspicious PowerShell / LOLBin Activity](playbooks/11-powershell-lolbin-activity.md)
12. [Web Application Attack (WAF/Logs)](playbooks/12-web-app-attack-response.md)
13. [DDoS Service Degradation](playbooks/13-ddos-service-degradation.md)
14. [Third-party / Supply-chain Compromise](playbooks/14-third-party-supply-chain-compromise.md)


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

- [01 AI LLM incident](siem-query-stubs/01-ai-llm-security-incident-response-siem-stubs.md)
- [02 Phishing](siem-query-stubs/02-phishing-email-triage-siem-stubs.md)
- [03 BEC](siem-query-stubs/03-business-email-compromise-siem-stubs.md)
- [04 Ransomware](siem-query-stubs/04-ransomware-response-siem-stubs.md)
- [05 Endpoint malware](siem-query-stubs/05-endpoint-malware-response-siem-stubs.md)
- [06 Impossible travel](siem-query-stubs/06-impossible-travel-signin-siem-stubs.md)
- [07 Brute-force](siem-query-stubs/07-bruteforce-credential-stuffing-siem-stubs.md)
- [08 Privileged misuse](siem-query-stubs/08-privileged-account-misuse-siem-stubs.md)
- [09 Exfiltration](siem-query-stubs/09-data-exfiltration-siem-stubs.md)
- [10 Cloud key exposure](siem-query-stubs/10-cloud-iam-key-exposure-siem-stubs.md)
- [11 PowerShell/LOLBins](siem-query-stubs/11-powershell-lolbin-activity-siem-stubs.md)
- [12 Web app attack](siem-query-stubs/12-web-app-attack-response-siem-stubs.md)
- [13 DDoS](siem-query-stubs/13-ddos-service-degradation-siem-stubs.md)
- [14 Supply chain](siem-query-stubs/14-third-party-supply-chain-compromise-siem-stubs.md)


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
