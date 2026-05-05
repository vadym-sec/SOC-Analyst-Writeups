# Security Operations Center (SOC) Fundamentals

## Overview
Introduction to SOC, its role, structure, and responsibilities in detection and responding to security incidents.

---

## What is a SOC
A SOC (Security Operations Center) is a team responsible for detection and response. Continuous monitoring is required to detect and respond to any security incident. 

---

## Three Pillars of a SOC

A SOC is built on three core pillars: People, Process, and Technology. These elements work together to ensure effective detection and response to security incidents.

- **People** – security analysts and specialists who monitor, investigate, and respond to threats  
- **Process** – defined procedures and workflows for detecting, analyzing, and handling incidents  
- **Technology** – tools and systems (e.g., SIEM, EDR) used to collect, analyze, and act on security data  

All three pillars must work together to build a mature and effective SOC environment.

## SOC Team Roles

### Tier 1 (L1) Analyst
- Monitor alerts  
- Perform basic triage  
- Escalate incidents  

### Tier 2 (L2) Analyst
- Deep investigation  
- Correlate events  
- Identify attack patterns  

### Tier 3 (L3) Analyst
- Threat hunting  
- Advanced analysis  
- Develop detection rules

### Security Engineer
- Deploy security solutions
- Configure security solutions

### Detection Engineer
- Build security rules
- Provide detection policy

### SOC Manager
- Manage the processes
- Lead the SOC team
- Remein in contact with CISO

The roles in the SOC team can increase or decrease depending on the size and criticality of the organizations.

## Process

SOC processes define how security events are handled from detection to resolution. They ensure consistency, prioritization, and proper escalation of security incidents.

### Alert Triage
Alert triage is the first step in SOC response. It focuses on analyzing incoming alerts to determine their severity and relevance. Analysts prioritize alerts based on impact and urgency using the **5 Ws**:

- **What?** – What happened? (e.g., malware detected on a host)  
- **When?** – When did the event occur?  
- **Where?** – Where was it detected (system, host, or location)?  
- **Who?** – Which user or account is involved?  
- **Why?** – Why did it happen (initial root cause or context)?

This step helps SOC analysts decide whether an alert is a false positive or a real security incident.

---

### Reporting
Confirmed or suspicious alerts are escalated as tickets to higher-level analysts or relevant teams. Reports should include:

- All 5 Ws  
- Analysis of the event  
- Evidence (logs, screenshots, etc.)

Proper reporting ensures clear communication and faster incident handling.

---

### Incident Response & Forensics
High-severity alerts may trigger incident response procedures. These involve structured actions to contain, investigate, and remediate threats.

In some cases, forensic analysis is performed to identify the root cause by examining system and network artifacts.

## Technology

Technology in a SOC refers to the security tools and solutions used to detect, analyze, and respond to threats. Even with strong People and Processes, effective security operations are not possible without proper technology.

These tools help reduce manual effort by centralizing data from different systems and enabling automated detection and response.

---

### SIEM (Security Information and Event Management)
SIEM solutions collect and correlate logs from multiple sources across the network (servers, endpoints, applications, etc.).

Key functions:
- Centralized log collection
- Rule-based detection of suspicious activity
- Correlation between different log sources
- Alert generation when matching detection rules

Modern SIEM systems also include:
- User behavior analytics (UEBA)
- Threat intelligence integration
- Machine learning-based detection improvements

SIEM primarily provides **detection and visibility**, not direct response.

---

### EDR (Endpoint Detection and Response)
EDR operates at the endpoint level (workstations, servers).

Key functions:
- Real-time and historical endpoint monitoring
- Deep visibility into process and file activity
- Detection of malicious behavior on devices
- Automated response actions (e.g., isolation of a host)

EDR is essential for detailed endpoint investigation and response.

---

### Firewall
A firewall protects the network by controlling incoming and outgoing traffic between trusted and untrusted networks.

Key functions:
- Traffic filtering based on rules
- Blocking unauthorized connections
- Monitoring network activity
- Detecting and preventing suspicious traffic before it reaches internal systems

---

### Other Security Technologies
SOC environments may also include:
- Antivirus / EPP (Endpoint Protection Platform)
- IDS/IPS (Intrusion Detection/Prevention Systems)
- XDR (Extended Detection and Response)
- SOAR (Security Orchestration, Automation and Response)

Each tool plays a specific role depending on the organization’s infrastructure and threat landscape.

## Analyst Perspective 
A foundational understanding of Security Operations Center (SOC) structure, key roles and responsibilities, and how they collaborate to support efficient threat detection and incident response processes.



