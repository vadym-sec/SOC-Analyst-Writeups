# Digital Forensics Fundamentals

## Overview

This writeup covers the fundamental concepts of Digital Forensics and its role in investigating cyber crimes involving digital devices.

The focus is on understanding:
- What digital forensics is  
- How digital evidence is collected and analyzed  
- The role of forensic procedures during investigations  
- How forensic findings support legal and incident response processes  

The writeup also explores how investigators examine devices such as laptops, mobile phones, hard drives, and USB devices to identify evidence related to criminal activity.

Topics covered include:
- Evidence collection and preservation  
- Basic forensic investigation workflow  
- Analysis of digital artifacts  
- Reporting and documentation of findings  
- The importance of maintaining evidence integrity  

This room provides foundational knowledge of how digital forensic investigations are conducted in real-world scenarios.

## Digital Forensics Process

Digital forensics follows a structured investigation methodology defined by the National Institute of Standards and Technology (NIST). The process is divided into four main phases that help investigators securely collect, examine, analyze, and report digital evidence.

### Collection
The collection phase focuses on identifying and securely acquiring evidence from digital devices such as computers, laptops, mobile phones, USB drives, and storage media.

Important goals of this phase:
- Preserve the integrity of the original evidence  
- Prevent data tampering  
- Properly document collected items and their details  

---

### Examination
During examination, investigators filter and extract relevant data from the collected evidence.

Examples:
- Filtering files by specific dates and times  
- Extracting activity related to a particular user account  
- Identifying files or artifacts relevant to the investigation  

This phase helps reduce unnecessary data before deeper analysis begins.

---

### Analysis
The analysis phase involves correlating evidence and reconstructing events related to the investigation.

The goal is to:
- Understand what happened  
- Identify suspicious or malicious activity  
- Build a chronological timeline of events  
- Draw conclusions based on available evidence  

This is considered one of the most critical stages of digital forensics.

---

### Reporting
In the final phase, investigators prepare a detailed report containing:
- Investigation methodology  
- Findings and evidence  
- Analysis results  
- Recommendations or conclusions  

Reports should be clear and understandable for both technical and non-technical audiences.

---

## Types of Digital Forensics

Different investigations require different forensic techniques and tools depending on the type of evidence involved.

### Computer Forensics
Focuses on investigating computers and storage devices to recover and analyze digital evidence.

### Mobile Forensics
Involves extracting and analyzing data from mobile devices such as:
- Call logs  
- Messages  
- GPS data  
- Application activity  

### Network Forensics
Focuses on analyzing network traffic and logs to investigate suspicious or malicious network activity.

### Database Forensics
Investigates databases for evidence of unauthorized access, data modification, or data theft.

### Cloud Forensics
Focuses on investigating evidence stored in cloud environments and services.

### Email Forensics
Analyzes emails to identify phishing attempts, fraud, malicious attachments, or suspicious communication activity.
## Evidence Acquisition

Evidence acquisition is one of the most critical parts of digital forensics. Investigators must collect and preserve digital evidence securely while ensuring that the original data remains unchanged.

Proper evidence handling is essential for maintaining the integrity and reliability of an investigation.

---

### Proper Authorization
Before collecting any evidence, investigators must obtain authorization from the appropriate authorities.

This is important because:
- Digital evidence may contain sensitive or private information  
- Unauthorized collection may make evidence inadmissible in court  
- Investigations must remain within legal boundaries  

Proper authorization ensures that forensic procedures are legally compliant.

---

### Chain of Custody
A chain of custody is a formal document used to track evidence throughout the investigation process.

It helps maintain accountability and preserves the integrity of the evidence by documenting:
- Description of the evidence  
- Individuals who collected or accessed it  
- Date and time of collection  
- Evidence storage locations  
- Access history and handling records  

Maintaining a proper chain of custody helps prove that the evidence has not been altered or tampered with.

---

### Use of Write Blockers
Write blockers are forensic tools used to prevent changes to the original evidence during acquisition.

Without a write blocker, connecting a storage device to a forensic workstation may unintentionally modify:
- File timestamps  
- Metadata  
- System information  

A write blocker ensures that the original device remains unchanged while investigators safely collect forensic copies for analysis.

## Windows Forensics

Windows systems are among the most common sources of digital evidence in forensic investigations. Since many cyber crimes involve personal computers or laptops, investigators frequently analyze Windows operating systems during forensic cases.

During evidence acquisition, investigators typically create forensic images of the system. These images are exact bit-by-bit copies of the original data and are used for analysis without modifying the source device.

---

## Types of Forensic Images

### Disk Image
A disk image contains all data stored on the system’s storage devices such as HDDs or SSDs.

This data is considered **non-volatile**, meaning it remains available even after the system is powered off or restarted.

Examples of data found in disk images:
- Documents and media files  
- Browser history  
- Installed applications  
- System files  
- Deleted files and metadata  

Disk images are essential for investigating long-term activity on a system.

---

### Memory Image
A memory image captures the contents of the system’s RAM.

This data is **volatile**, meaning it is lost after shutdown or restart. Because of this, memory acquisition is often prioritized during investigations.

Examples of volatile data:
- Running processes  
- Active network connections  
- Open files  
- Loaded malware  
- User sessions and credentials  

Memory analysis is extremely valuable for identifying active threats and live system activity.

---

## Common Windows Forensics Tools

### FTK Imager
FTK Imager is a widely used forensic tool for acquiring and analyzing disk images.

Key features:
- Disk image acquisition  
- Support for multiple image formats  
- File and directory analysis  
- User-friendly graphical interface  

---

### Autopsy
Autopsy is an open-source digital forensics platform used for analyzing disk images.

Key capabilities:
- Keyword searching  
- Deleted file recovery  
- Metadata analysis  
- File extension mismatch detection  
- Timeline and artifact analysis  

---

### DumpIt
DumpIt is a lightweight command-line utility used to capture memory images from Windows systems.

It is commonly used during live forensic investigations to preserve volatile data.

---

### Volatility
Volatility is a powerful open-source framework used for memory forensics.

It supports analysis of memory images from multiple operating systems, including:
- Windows  
- Linux  
- macOS  
- Android  

Volatility uses plugins to extract and analyze forensic artifacts such as:
- Running processes  
- Network connections  
- DLLs and drivers  
- Malware indicators  

---

Both disk and memory forensics play an important role in understanding system activity and identifying evidence during digital investigations.

## Note

It is important to understand that some forensic tools and techniques can also be used by attackers for reconnaissance and information gathering. Because of this, organizations and individuals should minimize unnecessary metadata and traces that could expose valuable information to potential attackers.

### Analyst Perspective

From an analyst’s perspective, metadata can provide valuable context during an investigation. Information such as timestamps, device details, software used, GPS coordinates, or file properties may help reconstruct events and identify suspicious activity.

This task also showed how small details left inside files can become useful evidence during forensic analysis. Understanding how to examine metadata helps analysts build timelines, verify activity, and gather additional context during investigations.

At the same time, it is important to recognize that attackers may also use metadata for reconnaissance and information gathering. Because of this, organizations should minimize unnecessary traces and exposed information whenever possible.
