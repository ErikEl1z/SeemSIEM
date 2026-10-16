# Azure SIEM Architecture

## Overview
This document explains the architecture used to ingest Windows telemetry into Azure Log Analytics using Sysmon, the Azure Monitor Agent (AMA), and a custom Data Collection Rule (DCR). The goal is to demonstrate how raw Windows event logs become structured, queryable data inside Azure.

---

## Pipeline Diagram

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

---

## Component Roles

### Windows VM
Generates raw logs:
- Security  
- System  
- Application  
- Sysmon  

These logs form the foundation of the SIEM pipeline.

---

### Sysmon
Provides enhanced telemetry including:
- Process creation (Event ID 1)  
- Network connections (Event ID 3)  
- File modifications  
- Registry changes  

Sysmon Operational logs must be visible in Event Viewer before ingestion.

---

### Event Viewer
Used to confirm logs exist locally before they are collected by AMA.

---

### Azure Monitor Agent (AMA)
Responsible for:
- Collecting logs from the VM  
- Sending Heartbeat signals  
- Forwarding telemetry to Log Analytics  

AMA replaces the legacy Microsoft Monitoring Agent (MMA).

---

### Data Collection Rule (DCR)
Defines:
- Which log channels to ingest  
- How logs map to tables  
- Which resources the rule applies to  

The VM must be bound to the DCR for ingestion to occur.

---

### Log Analytics Workspace
Stores logs in tables such as:
- `Event`  
- `SecurityEvent`  
- `Sysmon`  
- `Heartbeat`  

Workspace queries confirm ingestion and allow analysis.

---

### KQL
Used to:
- Validate ingestion  
- Explore logs  
- Perform basic SIEM analysis  
