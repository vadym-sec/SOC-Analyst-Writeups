# SOC Workbooks and Lookups

## Overview

This writeup deep-dives into the structured detection and automation mechanisms that power modern Security Operations Centers. Specifically, I explore how enterprise defenders utilize **Workbooks (Playbooks)** to standardize complex incident response flows and leverage **Lookups** to enrich raw log data with external threat intelligence and internal business context.

The focus is on understanding:
* How to pivot from chaotic, ad-hoc log analysis to predictable, playbook-driven triage.
* The architectural role of asset inventories and lookup tables in reducing false-positive rates.
* Technical implementation strategies within SIEM/EDR platforms to accelerate an analyst's investigative velocity using corporate network diagrams.

Topics covered include:
* Workbook design logic and automated alert handling.
* Data enrichment methodologies using lookups (Asset lists, Active Directory context).
* Practical workflow optimization to minimize Mean Time to Detect (MTTD) and Mean Time to Respond (MTTR).

By mastering these architectural tools, a defender transitions from a reactive ticket-closer to an engineering-minded analyst capable of handling real-world incidents at scale.

---

## Understanding the Analyst's Dilemma: Raw Logs vs. Structured Context

When an alert fires on a SIEM dashboard, it arrives as a collection of raw strings: an isolated IP address, a random hostname, or an ambiguous security identifier (SID). In an enterprise environment hosting thousands of endpoints and users, analyzing these data points in a vacuum is an operational bottleneck. 

To determine whether an event is a malicious intrusion or a benign administrative task, I must quickly answer critical contextual questions:
* Is this IP address part of the critical production subnet, or is it an isolated guest Wi-Fi client?
* Does this user account belong to a third-party contractor, or is it a high-privilege domain administrator?
* Is this system a legacy server undergoing scheduled testing, or is it a core Active Directory controller?

Answering these questions manually by pivoting across separate IT ticketing systems, HR databases, and network topology spreadsheets drastically increases response latency. **SOC Workbooks** and **Lookups** resolve this dilemma by programmatically injecting this crucial context directly into the primary investigative workspace.

[Raw SIEM Alert] ──> [Programmatic Lookup Engine] ──> [Workbook Execution] ──> [Targeted Action]
│                           │                           │
(Isolated IP/SID)         (Enriched Context:        (Standardized Step-by-Step     (Isolation or
Asset Criticality)            Triage Playbook)             Closure)


## The Engineering Mindset: Transitioning to Playbook-Driven Triage

As a specialist entering a production environment, it is tempting to view alert investigation as a creative, purely ad-hoc hunting exercise. However, high-volume environments require strict predictability. If three different analysts triage the exact same phishing or ransomware alert using three different manual methods, the security posture of the organization becomes inconsistent.

* **Workbooks (Playbooks/Runbooks):** Act as the codified intelligence of the SOC. They are structured, step-by-step instructions customized for specific alert categories (e.g., Brute Force, Malicious File Download, Anomalous Privileged Access). They dictate exactly what logs to query, what artifacts to extract, and what conditions mandate immediate escalation.
* **Lookups:** Serve as the data enrichment backend. They map static or dynamic reference data—such as known malicious IP feeds, internal employee registries, or network asset boundaries—directly to active log schemas. This allows the SIEM to automatically append variables like `User_Role` or `Subnet_Zone` to an incoming log stream before the analyst even opens the ticket.

By coupling structured playbooks with rich lookup data, a defender minimizes cognitive overload, enforces consistency across all shifts, and ensures that critical environmental nuances are never overlooked during the high-stress triage phase.

---

## Operational Contextualization: Parsing Identity & Asset Variables

To demonstrate the precision required in an enterprise triage workflow, let’s analyze a raw scenario: an alert indicates that user `G.Baker` authenticated into server `HQ-FINFS-02`, downloaded `Financial Report US 2024.xlsx`, and shared it with `R.Lund`. 

To an untrained eye, this looks like an immediate data exfiltration incident. However, I do not panic; I immediately cross-reference the raw indicators against our **Identity** and **Asset Inventories** to map out the baseline.

### 1. Identity Inventory: Decoding the Human Element

An Identity Inventory is a centralized, authoritative registry of all human operators (user accounts) and automated processes (service accounts) within the infrastructure. 

When we parse our raw usernames through the identity lookup, the context shifts entirely:
* **`G.Baker`** is revealed to be **Gregory Baker, the Chief Financial Officer (CFO)**. His role natively dictates access to high-level financial data.
* **`R.Lund`** is **Raymond Lund, a US Financial Adviser**. This explains why a financial document would be shared with him—there is a clear, legitimate business intersection between a CFO and a financial adviser.

#### Strategic Sources of Identity Data
In a production SOC environment, we programmatically ingest these lookups from specific authoritative systems:
* **Active Directory / Entra ID:** The primary authentication backbone, providing Group Policy objects, organizational units (OUs), and cryptographic security identifiers (SIDs).
* **Identity Providers (IdP):** Okta or Google Workspace, which log modern SAML/OAuth SSO cloud authentications and provide geographic context.
* **HR Information Systems (HRIS):** Platforms like BambooHR or SAP. While non-technical, integration with HR databases allows the SIEM to automatically flag indicators like *“Alert triggered by an employee currently on explicit garden leave or terminated status.”*

### 2. Asset Inventory: Verifying Infrastructure Criticality

An Asset Inventory (Asset Lookup) defines the exact network posture, operating system baseline, physical/logical location, and business purpose of every piece of hardware on the wire.

When we parse the target host **`HQ-FINFS-02`** through our asset lookup module, we uncover critical telemetry:
* **Host Attributes:** It is a Windows Server 2022 instance located in the UK Datacenter (`172.16.15.89`).
* **Business Function:** It is designated as the *Central IT File server for financial records*. 

Because `G.Baker` (the CFO) accessed a file server explicitly built for financial records, the *initial access* alignment maps perfectly to a benign operational profile.

#### Strategic Sources of Asset Data
* **Endpoint Detection & Response (EDR) / SIEM Telemetry:** Agents (e.g., CrowdStrike, Elastic) continuously poll the host for OS versioning, active MAC addresses, and local interface configurations.
* **Mobile Device Management (MDM):** Microsoft Intune or Jamf Pro, which validate whether the connecting endpoint is a managed corporate asset or an unauthorized, uncompliant personal device.

### The Forensic Synthesis: Building the Matrix

By merging these lookup tables, we transform raw log strings into an actionable security posture map:

| Forensic Vector | Raw Telemetry Variable | Enriched Contextual Value (From Lookups) | Operational Assessment |
| :--- | :--- | :--- | :--- |
| **Source Identity** | `G.Baker` | Gregory Baker (CFO, Europe/UK) | High-privilege data owner; behavior aligns with executive mandate. |
| **Target Asset** | `HQ-FINFS-02` | 172.16.15.89 (UK Finance File Server) | Authorized repository for the requested file type. |
| **Data Artifact** | `Financial Report...` | High-Value Financial Asset | Expected data class given the Source and Target context. |
| **Destination Identity** | `R.Lund` | Raymond Lund (US Financial Adviser) | Legitimate business consumer of financial reports. |

---

## Network Diagrams: The Architectural Blueprint of Triage

When isolating a threat, looking up user identities and hostnames only provides half the defensive picture. To truly understand how an attack can progress, I must master the structural layout of the infrastructure by utilizing the **Corporate Network Diagram**.

Consider a live scenario analyzed from firewall logs:
* **08:00:** An external IP (`103.61.240.174`) is repeatedly hitting the perimeter firewall via port `TCP/10443`.
* **08:23:** Perimeter logs show a successful authentication sequence, and that external IP is translated via NAT to an internal lease address: `10.10.0.53`.
* **08:25:** This new internal IP immediately begins mapping out the `172.16.15.0/24` network range, but connections are dropped.
* **08:32:** The asset pivots and begins actively scanning the `172.16.23.0/24` network range, indicating an ongoing attack.

Without an architectural map, these are just random numbers flashing on a SIEM. However, overlaying these logs onto the network diagram makes the attacker's footprint instantly visible.



### Reconstructing the Attacker's Path

By tracing these specific subnets and ports on the network diagram, I can reverse-engineer the adversary's strategy in real-time:

1. **The Ingress Point (Perimeter):** Port `TCP/10443` maps directly to the external gateway (`vpn.tryhatme.thm`). The initial repetitive connections at 08:00 indicate a targeted **VPN brute-force** or password-spraying campaign.
2. **Initial Access & IP Assignment:** The translation to `10.10.0.53` at 08:23 confirms the adversary successfully compromised an employee's credentials, bypassed the authentication layer, and was dropped straight into the **VPN Subnet pool**.
3. **Lateral Reconnaissance Phase 1:** The sweep of `172.16.15.0/24` reveals the attacker’s primary objective. The network map identifies this range as the **Database Subnet** (the crown jewels). Fortunately, the lack of open ports indicates internal firewall segregation rules blocked unauthorized traffic moving from the VPN pool into the DB zone.
4. **The Tactical Pivot:** Blocked at the database perimeter, the attacker switches targets at 08:32 and begins scanning `172.16.23.0/24`. The diagram identifies this as the **Office Subnet**. The adversary is now hunting for low-hanging fruit—vulnerable employee workstations—to use as a secondary jumping-off point.

[External Attacker: 103.61.240.174]
│
▼ (Brute Force - Port 10443)
┌───────────────────┐
│    VPN Gateway    │ ──► Assigned Internal IP: 10.10.0.53
└───────────────────┘
│
┌────────┴────────┐
▼ (Scan Blocked)  ▼ (Active Scan Ongoing)
┌──────────────┐  ┌──────────────┐
│ Database Zone│  │ Office Zone  │
│172.16.15.0/24│  │172.16.23.0/24│
└──────────────┘  └──────────────┘

## Standardizing Triage: The Engineering Power of SOC Workbooks

With identity inventory, asset databases, and network topology charts at my disposal, I can easily aggregate critical context around any user, host, or IP address. However, gathering data is only half the battle; the core challenge is using that data to form a definitive, accurate verdict. 

For routine events, triage is straightforward. But when facing advanced attack vectors, the investigation can quickly splinter into dozens of operational steps. Missing a single sub-query or failing to verify an anomaly can lead to a catastrophic blind closure of a real threat. 

To enforce absolute consistency across all shifts and eliminate analytical errors, I rely on **SOC Workbooks** (also known as *Playbooks*, *Runbooks*, or *Structured Workflows*).

These workbooks act as the codified intelligence of senior defenders, providing junior specialists with a strict, conditional roadmap for investigating specific threat categories. Following these playbooks is not just a recommendation—it is an operational mandate that guarantees high-fidelity results.

---

## Architectural Breakdown: Unusual Login Location Workflow

To visualize how a workbook structures an investigation, consider this production-grade flowchart for triaging an **Atypical/Unusual Login Location** alert. 

![Unusual Login Location Investigation Workbook Workflow](https://github.com/vadym-sec/SOC-Analyst-Writeups/blob/main/screenshots/SOC_Workbook.png)

This logical framework splits the analyst's investigation into three core operational phases, preventing any cognitive gaps or premature conclusions:

### Phase 1: The Enrichment Stage
Before I run a single query inside the SIEM tracking user activity, I must establish environmental and external baselines. This stage relies entirely on automated or manual programmatic lookups:
* **Identity Enrichment:** I query HR and Identity access repositories (like BambooHR or Entra ID) to extract the user's official corporate location and operational role.
* **Threat Intelligence Ingestion:** The source login IP is cross-referenced against global reputation intelligence feeds to check for known malicious behavior.
* **Anonymization Detection:** I parse the IP through lookups to check if it matches commercial VPN pools, Tor exit nodes, or hosting provider data centers (e.g., AWS, DigitalOcean).

### Phase 2: The Investigation Stage
Once the baseline parameters are established, the workbook shifts the analytical logic into the active SIEM repository (such as Splunk or Microsoft Sentinel):
* **Contextual Evaluation:** If the IP reputation is confirmed as inherently malicious, the workflow immediately routes to an escalation path.
* **Behavioral Deep-Dive:** If the IP is clean but anomalous, I execute specific hunting playbooks (e.g., pulling a 90-day "Login Timeline" dashboard). I scan for concurrent high-risk actions executed immediately post-authentication, such as unexpected Multi-Factor Authentication (MFA) registration overrides or bulk privilege updates.
* **Temporal and Spatial Verification:** The workflow forces me to audit the exact time delta of the access. Is the login occurring during the user's standard operational hours, or is it a geometric impossibility given their previous active session?

### Phase 3: The Escalation & Remediation Stage
The final stage defines the deterministic actions mandated by the accumulated evidence:
* **The False Positive Path:** If the audit confirms the user is simply traveling on an authorized corporate business trip and no malicious actions occurred post-login, the alert is safely closed with documented evidence.
* **The Out-of-Band Validation Path:** If the location is atypical but inconclusive, the workbook dictates contacting the employee directly—utilizing alternative, secure out-of-band communication lines (like a cellular call) to verify session intent.
* **The True Positive Escalation:** If the indicators point to unauthorized session hijacking or credential compromise, I document the findings using the "Five Ws" architecture and shift ownership to Tier 2/DFIR for immediate host isolation and credential revocation.

* ---

## Tactical Notes: Maintaining Architecture in the Trenches

In a production environment, even the most beautifully designed workbook will fail if the underlying data layer is corrupted or out of date. Having spent time triaging alerts and optimizing workflows, I have compiled these core operational rules for engineering-minded defenders:

* **Treat Lookups as Code (LaC):** Static asset inventories stored in isolated `.csv` files are a massive risk vector. If an infrastructure team spins up a new cloud database and forgets to update the spreadsheet, that asset becomes a critical blind spot. Lookups must be dynamically synced via automated API integrations directly from the Source of Truth (e.g., Active Directory, AWS Asset Inventory, Azure Graph API).
* **The "Zero-Trust" Diagram Validation:** A network diagram is an idealized version of reality. When an alert fires, never trust the PDF diagram blindly. Always cross-reference the allowed paths on the map with live firewall connections (`DROP` vs. `ACCEPT` logs) and active NetFlow data. If the diagram says a zone is isolated, but your logs show inbound traffic on port 445, trust the logs and hunt for misconfigurations.
* **Workbook Modularization:** Do not build a massive, monolithic 50-step playbook for every single specific alert. Instead, build modular sub-workflows (e.g., an "IP Enrichment Module" or an "Account Lockout Isolation Module") that can be dynamically called by different high-level workbooks. This simplifies maintenance and accelerates SOAR playbook development.

---

## Analyst Perspective: Engineering the Shield

From my perspective, the true dividing line between a junior tier-1 "ticket closer" and an elite SOC analyst is how they view **Workbooks** and **Lookups**. 

A passive analyst looks at a workbook as a rigid, bureaucratic checklist that they have to mindlessly click through to meet their SLA metrics. An elite analyst views a workbook as a **living piece of software architecture** that must be continuously optimized, refactored, and audited against the evolving threat landscape.

When I dive into an investigation using these tools, my mindset is driven by three core operational philosophies:

### 1. Context is the Ultimate Force Multiplier
Raw logs are just technical noise. An IP address like `10.10.0.53` means absolutely nothing until you overlay it with an Asset Inventory and a Network Diagram. The second I can programmatically see that this IP belongs to the VPN pool, and it is attempting to scan the Database Subnet, the noise instantly crystallizes into a high-priority lateral movement matrix. Speed in the SOC is dictated by how fast you can enrich raw data into actionable context.

### 2. Hunt for the "Smoke Screens"
When automation and workbooks streamline the queue, they also reveal tactical patterns. Sophisticated threat actors know that SOCs rely on automated playbooks. A common adversary technique is to intentionally trigger a massive flood of noisy, low-level alerts (like external brute-force attempts) to force your workbooks into high execution, distracting the human layer. While the dashboard is screaming about routine noise, I always look for the single, quiet, un-playbooked anomaly hidden in the background—because that is where the real intrusion is taking place.

### 3. Fail Fast, Automate Faster
If I find myself manually executing the same query more than three times during a shift—whether it is checking an IP reputation on VirusTotal or pulling an asset owner from an internal database—I view that as a failure of operational efficiency. My goal is to push our engineering team to programmatically ingest that specific step into a lookup or a SOAR automation rule. Every second saved on manual triage is a second clawed back from the adversary’s dwell time. 

Defensive operations are an asymmetric war of data velocity. By mastering workbooks and lookups, we don't just close tickets faster—we break the attacker's automation and deny them the mobility they need to succeed.
