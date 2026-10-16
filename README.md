# SeemSIEM
Azure SIEM buildout

# Azure SIEM Build: Windows Telemetry → AMA → DCR → Log Analytics → KQL

This project demonstrates a full end‑to‑end Azure SIEM ingestion pipeline using a Windows VM, Sysmon, the Azure Monitor Agent (AMA), Data Collection Rules (DCR), and Log Analytics Workspace validation through KQL. The goal is to show how raw Windows telemetry becomes structured, queryable security data inside Azure.

This lab is designed as a practical, beginner‑friendly SIEM engineering project suitable for DFIR, cloud security, and SOC analyst portfolios.

---

## 📌 Objectives

- Deploy a Windows VM and generate local event logs  
- Install and validate Sysmon for enhanced telemetry  
- Install and validate the Azure Monitor Agent (AMA)  
- Build a custom Data Collection Rule (DCR)  
- Ingest Windows Security, System, Application, and Sysmon logs  
- Validate ingestion using KQL queries  
- Document the pipeline with screenshots and architecture diagrams  

---

## 🔨 Architecture Overview

Windows VM
↓
Sysmon + Windows Event Logs
↓
Azure Monitor Agent (AMA)
↓
Data Collection Rule (DCR)
↓
Log Analytics Workspace
↓

KQL Queries

### **Flow Explanation**
- **Windows VM** generates Security, System, Application, and Sysmon logs  
- **Sysmon** provides detailed process creation, network connections, and file modification telemetry  
- **Event Viewer** confirms logs exist locally before ingestion  
- **Azure Monitor Agent (AMA)** collects logs from the VM  
- **Data Collection Rule (DCR)** defines which logs to ingest  
- **Log Analytics Workspace** stores the ingested logs  
- **KQL** is used to validate ingestion and analyze events  

A full screenshot index is available in `/screenshots`.

---

## 📁 Repository Structure
/screenshots
/sysmon
/dcr
/ama
/eventviewer
/workspace
/kql

/docs
architecture.md
ingestion-flow.md
kql-examples.md

---

## 📖 KQL Validation Used

### **1. Confirm SecurityEvent ingestion**
```kql
Event
| where EventLog == "Security"
| sort by TimeGenerated desc
| take 50
```


### **2. Check Sysmon ingestion**
```kql
Event
| where Source == "Microsoft-Windows-Sysmon"
| sort by TimeGenerated desc
| take 20
```

### **3. Confirm Application/System logs
```kql
Event
| where EventLog in ("Application", "System")
| sort by TimeGenerated desc
| take 20
```

## **4. AMA Heartbeat check
```kql
Heartbeat
| sort by TimeGenerated desc
| take 10
```

### ** 5. General Event table sanity check
```kql
Event
| take 20
```

❓ What Each Query Validates

✔ Security logs
Confirms your DCR is correctly collecting the Security event channel and that Windows auditing logs are flowing into the Event table.

✔ Sysmon logs
Validates that Sysmon is installed, running, and mapped to the Microsoft-Windows-Sysmon/Operational channel in your DCR.
Shows process creation, network events, and other Sysmon telemetry.

✔ Application/System logs
Ensures baseline Windows logs (Application + System) are being ingested.
These confirm your DCR is collecting standard OS channels.

✔ Heartbeat
Confirms the Azure Monitor Agent (AMA) is alive, connected, and sending heartbeat signals to the workspace.
If Heartbeat stops → ingestion stops.

✔ Event table sanity check
Shows that the ingestion pipeline is functioning at a basic level.
If Event | take 20 returns nothing, your DCR or AMA binding is broken.
