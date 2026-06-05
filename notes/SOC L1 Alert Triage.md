## Alert Triage Fundamentals

### Overview

This writeup covers the fundamental concepts of Alert Triage and its role in investigating security incidents involving digital networks and systems.

The focus is on understanding:
* What a SOC alert is
* How alert fields, statuses, and classification work
* The role of triage procedures during investigations
* How alert findings support legal and incident response processes

The writeup also explores how investigators examine dashboards such as SIEMs to identify evidence related to malicious activity.

Topics covered include:
* Alert fields, statuses, and classification
* Alert triage workflows for L1 analysts
* Investigation and correlation of digital events
* Reporting and documentation of alert findings
* The importance of maintaining accurate alert classification

This writeup provides foundational knowledge of how alert triage investigations are conducted in real-world scenarios.

### From Events to Alerts

The alert generation process follows a structured pipeline that transforms raw system activity into targeted security notifications. 

An event must first occur within the infrastructure (e.g., a user login, process launch, or file download), which is then recorded as a log by the local operating system, firewall, or cloud provider. These distributed logs are subsequently shipped to a centralized security platform. Because a corporate infrastructure can generate millions of logs daily, security solutions use correlation rules to identify suspicious or anomalous event sequences. This automated filtering surfaces specific alerts, shielding analysts from manual log review and narrowing down millions of raw data points into a manageable queue of critical events.

### Alert Management Platforms

Different security technologies are deployed within an enterprise to aggregate, orchestrate, and track alerts through their lifecycle:

| Solution | Platform Examples | Description |
| :--- | :--- | :--- |
| **SIEM (Security Information and Event Management)** | Splunk ES, Elastic | Features robust, native alert management capabilities and serves as the primary centralized system for most modern SOC teams. |
| **EDR / NDR (Endpoint / Network Detection & Response)** | Microsoft Defender, CrowdStrike | Provides specialized dashboards focused on endpoint or network telemetry. In standard operations, these alerts are forwarded to a SIEM or SOAR for centralized management. |
| **SOAR (Security Orchestration, Automation, and Response)** | Splunk SOAR, Cortex SOAR | Used primarily by larger SOC environments to automatically aggregate, centralize, and orchestrate alerts across multiple security tools and vendors. |
| **ITSM (IT Service Management)** | Jira, TheHive | Dedicated ticketing systems customized for security operations to manage incident workflow tracking, task assignments, and audit trails. |

### SOC Roles in Alert Triage

Maintaining an efficient defensive pipeline relies on a clear division of operational responsibilities across the security team:

*   **SOC L1 Analysts:** Act as the first line of defense. They review active alert queues, isolate malicious indicators from normal behavior, and escalate validated threats.
*   **SOC L2 Analysts:** Receive escalated tickets from the L1 team to conduct in-depth forensic analysis, determine blast radius, and execute incident remediation steps.
*   **SOC Engineers:** Maintain and optimize the detection architecture, ensuring that incoming alerts contain sufficient metadata and context for efficient analysis.
*   **SOC Managers:** Oversee the operational metrics, tracking the speed and quality of the triage process to ensure critical events are handled within required SLAs.

### Alert Properties

Every security alert contains standardized properties that provide the foundational metadata required to begin an investigation. While the structural design varies across different SIEM or security vendors, the core properties remain consistent:

| № | Property | Description | Examples |
| :--- | :--- | :--- | :--- |
| **1** | **Alert Time** | Shows alert creation time. Alert usually triggers a few minutes after the actual event | Alert Time: March 21, 15:35<br>Event Time: March 21, 15:32 |
| **2** | **Alert Name** | Provides a summary of what happened, based on the detection rule's name | Unusual Login Location<br>Email Marked as Phishing<br>Windows RDP Bruteforce<br>Potential Data Exfiltration |
| **3** | **Alert Severity** | Defines the urgency of the alert, initially set by detection engineers, but can be altered by analysts if needed | Low / Informational<br>Medium / Moderate<br>High / Severe<br>Critical / Urgent |
| **4** | **Alert Status** | Informs if somebody is working on the alert or if the triage is done | New / Unassigned<br>In Progress / Pending<br>Closed / Resolved<br>And often other custom statuses |
| **5** | **Alert Verdict** | Also called alert classification, explains if the alert is a real threat or noise | True Positive / Real Threat<br>False Positive / No Threat<br>And often other custom verdicts |
| **6** | **Alert Assignee** | Shows the analyst that was assigned or assigned themselves to review the alert | Assignee can sometimes be called alert owner<br>Assignee takes responsibility for their alerts |
| **7** | **Alert Description** | Explains what the alert is about, usually in three sections on the right | The logic of the alert generating rule<br>Why this activity can indicate an attack<br>Optionally, how to triage this alert |
| **8** | **Alert Fields** | Provides SOC analysts' comments and values on which the alert was triggered | Affected Hostname<br>Entered Commandline<br>And many more, depending on the alert |

> 📌 **Note:** When evaluating alert properties, it is essential to account for potential time differences (such as latency between the actual event time and the alert generation time). Furthermore, assessing threat urgency should not rely solely on the default alert severity; analysts must contextualize the threat against the criticality of the targeted infrastructure. Developing a deeper understanding of internal company workflows, asset impact baselines, and specific operational conditions is vital for accurate threat evaluation.

### Picking the Right Alert

Every SOC team determines its own prioritization guidelines, typically automating the process by applying specific alert sorting logic within the SIEM or EDR platform. A generic, fundamental approach involves three sequential steps:

*   **Filter the alerts:** Analysts must ensure they do not select an alert that has already been reviewed or is actively being investigated by another teammate. The focus should remain strictly on new, unassigned, and unresolved alerts.
*   **Sort by severity:** Triage begins with critical alerts, followed by high, medium, and low severities. This ordering is established because detection engineers configure rules so that higher severity levels correlate with a greater likelihood of a real, high-impact threat.
*   **Sort by time:** Within the same severity tier, alerts should be sorted from oldest to newest. The underlying methodology assumes that an older breach implies the adversary has had more time to advance their attack chain (such as data exfiltration), whereas a newer alert may catch the adversary during initial discovery.

> 📌 **Note:** When evaluating alert properties, it is essential to account for potential time differences, such as latency between the actual event time and the alert generation time. Furthermore, assessing threat urgency should not rely solely on default alert severity. As a critical addition outside the scope of this room, analysts must always contextualize the threat against the actual tier and criticality of the targeted infrastructure. It is vital to look deeper than just what the SIEM or SOAR indicates, developing a thorough understanding of internal company workflows, asset impact baselines, and specific operational conditions to perform accurate threat evaluation.

### The Alert Triage Workflow

The process of reviewing a selected alert—frequently referred to as alert handling, processing, investigation, or analysis—follows a structured operational flow to ensure comprehensive coverage and clear ownership. 

### 1. Initial Actions

The initial steps establish clean ownership over the alert queue to avoid overlapping efforts with other analysts and prepare for deeper technical analysis:
*   **Assign ownership:** The analyst assigns the new alert to themselves to signal active ownership to the rest of the team.
*   **Update operational status:** The alert state is moved from *New* to *In Progress*.
*   **Contextual review:** The analyst reviews fundamental alert details, including the rule name, description, and key indicators, to understand the initial scope of the triggered event.

### 2. Investigation

This phase involves technical log analysis within the SIEM or EDR environment to determine the legitimacy of the activity. While advanced teams utilize structured Workbooks (runbooks/playbooks) for step-by-step guidance, standard investigative practices rely on the following core pillars:
*   **Identify the target scope:** Determine exactly what assets or identities are under threat (e.g., specific usernames, hostnames, cloud environments, network segments, or web applications).
*   **Analyze the core action:** Evaluate the specific behavior flagged by the detection rule, such as anomalous authentications, process executions, or inbound phishing indicators.
*   **Examine surrounding telemetry:** Review chronological log context by inspecting events immediately preceding and following the alert to identify potential secondary compromise or lateral movement.
*   **Leverage Threat Intelligence:** Cross-reference technical artifacts (IP addresses, domains, file hashes) against external threat intelligence platforms and internal baselines to validate assumptions.

### 3. Final Actions

The final phase determines the operational resolution of the event and ensures the investigation is properly documented for future reference:
*   **Determine the classification verdict:** Classify the event as either a verified security threat (**True Positive**) or benign administrative/environmental noise (**False Positive**).
*   **Document findings:** Author a comprehensive technical comment detailing each analysis step, evidence uncovered, and the justification for the final verdict.
*   **Close the ticket:** Return to the primary monitoring dashboard, update the final classification parameters, and change the alert status to *Closed*.

### Note

It is essential to recognize that automated alert generation and scoring systems have inherent limitations. While a SIEM or SOAR platform streamlines the initial analysis pipeline, these tools cannot dynamically evaluate business context or internal infrastructure hierarchy on their own. A critical severity alert targeting an isolated testing environment may require far less urgent intervention than a medium or low severity alert pointing directly at an enterprise active directory controller or a database containing critical asset data. Relying blindly on default dashboard thresholds without cross-referencing corporate network topologies can result in catastrophic misprioritization during a live attack.

### Analyst Perspective

From an analyst's perspective, alert triage is a high-stakes puzzle where speed and rigorous accuracy must be balanced perfectly. Every piece of raw log metadata—from subtle time latencies to specific execution arguments—acts as a breadcrumb trail that allows us to reconstruct an attacker's precise footprint. Understanding how to peel back the layers of an alert, map the activity to an internal asset baseline, and trace peripheral events before and after a trigger time is what differentiates a reactive ticket-closer from an elite threat defender. 

To execute alert triage effectively in a production environment, I rely on a structured, highly repeatable technical workflow:

*   **Correlate Time Discrepancies:** Always calculate the delta between the exact time an event occurred on the host system and the time the alert reached the management interface to detect data delays or active tampering with system clocks.
*   **Establish Asset Blast Radius:** Instantly pivot to an asset management system to verify the network segment, business purpose, and vulnerability posture of the affected host before committing to an escalation path.
*   **Isolate and Profile Identities:** Evaluate the historical authentication behavior of the involved user account across enterprise firewalls, cloud environments, and internal networks to separate administrative noise from legitimate anomalies.
*   **Verify IOC Telemetry:** Extrapolate raw technical variables such as system file hashes, commands executed, or outbound network connections, and validate them using trusted threat intelligence repositories to confidently determine an accurate final verdict.
