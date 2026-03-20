# Web Application Attack (WAF/Logs) — SIEM query stubs

> Adapt fields/index names to your environment.

## Microsoft Sentinel (KQL)
```kusto
// Last 24h suspicious events for playbook scope
SecurityEvent
| where TimeGenerated > ago(24h)
| where EventID in (4624, 4625, 4688)
| take 100
```

## Splunk SPL
```spl
index=* earliest=-24h
| head 100
```

## Elastic (KQL)
```kql
@timestamp >= now-24h
```
