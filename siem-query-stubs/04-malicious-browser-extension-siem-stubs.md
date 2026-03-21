# Malicious Browser Extension — SIEM query stubs

## Microsoft Sentinel (KQL)
```kusto
BrowserExtensionEvents
| where TimeGenerated > ago(24h)
| where ExtensionRisk in ("high", "critical") or PermissionDelta has_any ("cookies", "webRequest", "tabs")
| project TimeGenerated, UserPrincipalName, DeviceName, ExtensionId, ExtensionName, PermissionDelta, Action
| order by TimeGenerated desc
```

## Splunk SPL
```spl
index=browser_audit earliest=-24h
(extension_risk=high OR extension_risk=critical OR permission_delta="*cookies*" OR permission_delta="*webRequest*")
| stats count values(permission_delta) as perms values(action) as actions by user device extension_id
| sort - count
```

## Elastic (KQL)
```kql
@timestamp >= now-24h and event.dataset : "browser.extension" and (extension.risk : ("high" or "critical") or extension.permission_delta : ("cookies" or "webRequest" or "tabs"))
```
