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

## Playbook Index (13)

1. [Phishing Email Triage and Response](playbooks/01-phishing-email-triage.md)
2. [Business Email Compromise (BEC)](playbooks/02-business-email-compromise.md)
3. [Ransomware Incident Response](playbooks/03-ransomware-response.md)
4. [Endpoint Malware Detection and Containment](playbooks/04-endpoint-malware-response.md)
5. [Impossible Travel / Suspicious Sign-in](playbooks/05-impossible-travel-signin.md)
6. [Brute-force / Credential Stuffing](playbooks/06-bruteforce-credential-stuffing.md)
7. [Privileged Account Misuse](playbooks/07-privileged-account-misuse.md)
8. [Data Exfiltration Suspected](playbooks/08-data-exfiltration.md)
9. [Cloud IAM Key Exposure](playbooks/09-cloud-iam-key-exposure.md)
10. [Suspicious PowerShell / LOLBin Activity](playbooks/10-powershell-lolbin-activity.md)
11. [Web Application Attack (WAF/Logs)](playbooks/11-web-app-attack-response.md)
12. [DDoS Service Degradation](playbooks/12-ddos-service-degradation.md)
13. [Third-party / Supply-chain Compromise](playbooks/13-third-party-supply-chain-compromise.md)

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
