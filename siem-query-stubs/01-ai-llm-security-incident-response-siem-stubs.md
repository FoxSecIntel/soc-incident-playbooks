# AI LLM Security Incident Response — SIEM query stubs

> Adapt fields, table names, and labels to your environment.

## Microsoft Sentinel (KQL)
```kusto
// Prompt-injection and leakage indicators from AI gateway logs
AiGatewayLogs
| where TimeGenerated > ago(24h)
| where RiskLabel in ("prompt_injection", "policy_bypass", "sensitive_data_leak")
| project TimeGenerated, UserId, SessionId, ModelName, PromptHash, RiskLabel, ActionTaken
| order by TimeGenerated desc
```

## Splunk SPL
```spl
index=ai_gateway earliest=-24h
(risk_label="prompt_injection" OR risk_label="policy_bypass" OR risk_label="sensitive_data_leak")
| stats count dc(session_id) as sessions values(model) as models by user_id risk_label action
| sort - count
```

## Elastic (KQL)
```kql
@timestamp >= now-24h and event.dataset : "ai.gateway" and risk.label : ("prompt_injection" or "policy_bypass" or "sensitive_data_leak")
```

## Optional correlation pivots
- Join with IAM logs for anomalous privileged actions after suspicious prompts.
- Join with RAG ingestion logs for recently added source documents.
- Join with key-management logs for unexpected token usage patterns.
