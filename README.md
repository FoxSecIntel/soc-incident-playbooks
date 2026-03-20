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


## v2 operational upgrades

- Added **RACI** matrix to each playbook.
- Added **severity-aware communication templates** (Medium/High/Critical).
- Added **war room one-page checklist** to each playbook.
- Added **SIEM query stubs** (Sentinel KQL, Splunk SPL, Elastic KQL): see [`siem-query-stubs/`](siem-query-stubs/).

## SIEM stub index

- [01 Phishing](siem-query-stubs/01-phishing-email-triage-siem-stubs.md)
- [02 BEC](siem-query-stubs/02-business-email-compromise-siem-stubs.md)
- [03 Ransomware](siem-query-stubs/03-ransomware-response-siem-stubs.md)
- [04 Endpoint malware](siem-query-stubs/04-endpoint-malware-response-siem-stubs.md)
- [05 Impossible travel](siem-query-stubs/05-impossible-travel-signin-siem-stubs.md)
- [06 Brute-force](siem-query-stubs/06-bruteforce-credential-stuffing-siem-stubs.md)
- [07 Privileged misuse](siem-query-stubs/07-privileged-account-misuse-siem-stubs.md)
- [08 Exfiltration](siem-query-stubs/08-data-exfiltration-siem-stubs.md)
- [09 Cloud key exposure](siem-query-stubs/09-cloud-iam-key-exposure-siem-stubs.md)
- [10 PowerShell/LOLBins](siem-query-stubs/10-powershell-lolbin-activity-siem-stubs.md)
- [11 Web app attack](siem-query-stubs/11-web-app-attack-response-siem-stubs.md)
- [12 DDoS](siem-query-stubs/12-ddos-service-degradation-siem-stubs.md)
- [13 Supply chain](siem-query-stubs/13-third-party-supply-chain-compromise-siem-stubs.md)
