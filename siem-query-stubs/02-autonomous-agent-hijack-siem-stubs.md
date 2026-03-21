# Autonomous Agent Hijack — SIEM query stubs

## Microsoft Sentinel (KQL)
```kusto
AgentToolAudit
| where TimeGenerated > ago(24h)
| where ActionRisk in ("high", "critical") or PolicyDecision == "blocked"
| project TimeGenerated, AgentId, ToolName, Action, Principal, PolicyDecision, Reason
| order by TimeGenerated desc
```

## Splunk SPL
```spl
index=agent_audit earliest=-24h
(action_risk=high OR action_risk=critical OR policy_decision=blocked)
| stats count values(tool_name) as tools values(action) as actions by agent_id principal
| sort - count
```

## Elastic (KQL)
```kql
@timestamp >= now-24h and event.dataset : "agent.audit" and (action.risk : ("high" or "critical") or policy.decision : "blocked")
```
