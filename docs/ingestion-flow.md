# Azure SIEM Ingestion Flow

This document outlines the full ingestion pipeline from local Windows logs to Azure Log Analytics.

---

## 1. Local Log Generation
The Windows VM produces Security, System, Application, and Sysmon logs.  
Event Viewer is used to confirm logs exist before ingestion.

---

## 2. Sysmon Installation
Sysmon is installed using a configuration file (e.g., SwiftOnSecurity config).  
Telemetry includes:
- Process creation  
- Network connections  
- File modifications  
- Registry changes  

Sysmon Operational logs must appear in Event Viewer.

---

## 3. Azure Monitor Agent (AMA)
AMA is installed via Azure Extensions.  
It provides:
- Heartbeat signals  
- Log forwarding  
- DCR enforcement  

AMA must show a healthy status before ingestion begins.

---

## 4. Data Collection Rule (DCR)
A custom DCR is created to ingest:
- Security  
- System  
- Application  
- Microsoft-Windows-Sysmon/Operational  

The VM is bound to the DCR.  
Once applied, AMA begins forwarding logs to the workspace.

---

## 5. Log Analytics Workspace
Logs flow into tables:
- `Event`  
- `SecurityEvent`  
- `Sysmon`  
- `Heartbeat`  

Workspace queries confirm ingestion.

---

## 6. KQL Validation
KQL queries validate:
- Security logs  
- Sysmon logs  
- Application/System logs  
- AMA heartbeat  
- General ingestion health  

These queries ensure the pipeline is functioning end‑to‑end.
