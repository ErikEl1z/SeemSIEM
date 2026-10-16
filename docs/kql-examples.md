# KQL Examples for Ingestion Validation

This document contains the KQL queries used to validate ingestion in the Azure SIEM pipeline.

---

## 1. Confirm SecurityEvent ingestion
```kql
Event
| where EventLog == "Security"
| sort by TimeGenerated desc
| take 50
```
## 2. Check Sysmon ingestion
```kql
Event
| where Source == "Microsoft-Windows-Sysmon"
| sort by TimeGenerated desc
| take 20
```

## 3. Confirm Application/System logs
```kql
Event
| where EventLog in ("Application", "System")
| sort by TimeGenerated desc
| take 20
```
## 4. AMA Heartbeat check
```kql
Heartbeat
| sort by TimeGenerated desc
| take 10
```

## 5. General Event 
```kql
Event
| take 20
```

What Each Query Validates

✔ Security logs
Confirms DCR is collecting the Security channel.

✔ Sysmon logs
Validates Sysmon Operational channel mapping.

✔ Application/System logs
Confirms baseline OS logs are ingested.

✔ Heartbeat
Confirms AMA is alive and connected.

✔ Event table sanity check
Confirms ingestion pipeline is functioning.able sanity check
