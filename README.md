# 🛡️ SOC Alert Triage – Multi-Stage Phishing to Data Exfiltration (SOC Simulator)

## 📌 Overview

This project documents a simulated **SOC alert triage and incident investigation** completed in a SOC Simulator environment using Splunk logs, endpoint telemetry, Sysmon Logs, and network activity.

The scenario mirrors a realistic SOC investigation involving:
- A successful phishing attack
- Multiple alerts tied to the same user and host
- Correlation across endpoint, network, and file activity
- Identification of post-compromise behavior
- Escalation based on confirmed impact (data exfiltration)

Although alerts were generated individually, analysis was conducted at the incident level, reflecting real-world SOC workflows.

---

## 🔍 Environment & Tooling

- **SIEM:** Splunk Enterprise log interface  
- **Log Sources:**
  - Email telemetry
  - Endpoint process execution Sysmon logs
  - File system activity
  - Network / DNS activity
- **Scenario Type:** Phishing → Post-Exploitation → Data Exfiltration

---

## 🧠 Investigation Summary

During the simulation, I investigated compiling alerts in the SIEM dashboard, where I specifically identified 17 alerts generated over a short time window involving one user/machine. All alerts were tied to a single attack chain impacting one high-value user, the CEO.

The investigation revealed:
- Initial compromise via a phishing attachment delivered in a compressed .zip file, using attachment layering to obscure malicious content
- Execution of PowerShell tooling following the user interaction
- Unauthorized access to sensitive internal file shares, indicating post-compromise pivoting
- Active outbound communication using nslookup to blend the exfiltration traffic with normal DNS activity
- Data obfuscation using Base64 encoding and exfiltration achieved through PowerShell pipelines redirecting encoded data to an external source


Understanding the full scope required correlating alerts from several sources rather than treating them all as isolated events.

---

## 📂 Case Breakdown (High-Level)

### 🟢 Initial Access – Phishing

**Indicators Observed:**
- Phishing email sent from external domain 
- Malicious attachment: Zip file containing malicious PDF file
- High-value target - CEO

**Action Taken:**  
- Identified phishing email as the root cause  
- Correlated downstream alerts back to initial attachment execution  

**Outcome:**  
Confirmed initial access vector

---

### 🟡 Post-Compromise Activity

**Indicators Observed:**
- Download of external .ps1 file and execution of Powershell file
- Use of legitimate Windows binaries rdpclip.exe, net.exe, robocopy.exe
- Disconnection and remapping of a network drive to access an internal file share
- Unauthorized access to sensitive internal resources within file share

**Action Taken:**
- Correlated process execution and file access activity  
- Identified this as data staging behavior prior to the data exfiltration  

**Outcome:**  
Confirmed unauthorized internal access and preparation for data theft

---

### 🔴 True Positive – Escalated Incident (Data Exfiltration)

**Key Indicators:**
- DNS used to obfuscate outbound communication as standard DNS traffic using nslookup.exe
- PowerShell pipelines establishing external connections
- Use of powercat.ps1 for tunneling

**Why This Required Escalation:**
- Confirmed compromise of endpoint
- Unauthorized access to sensitive financial data
- Verified outbound data transfer to external source

**Action Taken:**
- Correlated endpoint, file, and network activity  
- Identified affected user, host, and destination source  
- Classified activity as active data exfiltration  
- Escalated for immediate containment and response  

**Outcome:**  
Incident escalated due to confirmed impact

---

## 🧩 Correlation & Analysis Highlights

- Correlated 17 alerts into a single incident narrative
- Mapped Attack progression across attack stages:
  - Phishing → Execution → Discovery → Staging → Exfiltration
- Distinguished between:
  - Suspicious activity
  - Confirmed malicious behavior
- Used behavior and impact to drive decisions, not alert volume

---

## 📉 Lessons Learned


### 1. Tool Familiarity vs Behavioral Analysis

Some observed commands and tooling were unfamiliar at first glance.

**Takeaway:**  
Even without immediate recognition of every command, attacker behavior provided enough signal to make correct escalation decisions.  
Quick researching of unknown tools revealed known offensive utilities ( powercat.ps1, a PowerShell-based netcat-style tool ), this helped reinforce the assessment the user/machine was compromised.
This incident reaffirmed that recognizing attacker behavior is more critical than memorizing specific tools or IOCs. Tool names and indicators can be researched quickly, but the ability to question unusual command execution and investigate why activity is occurring in uncommon contexts was the more valuable skill in this case.
This reinforced my confidence in trusting behavioral indicators when making escalation decisions under time pressure.


---

### 2. Visibility Gaps Matter

Indicators of privilege escalation or lateral movement were suspected but not fully visible in the provided logs.

**Takeaway:**  
Documenting visibility gaps is critical, both for improving future detections and for maintaining thorough, honest reporting. While I was unable to identify logs showing the exact privilege escalation, strong correlation across events helped confirm the continuation of the attack and supported the overall assessment of malicious behavior.

---

## 🧪 Skills Demonstrated

- SOC alert triage and prioritization
- Correlation of multiple alerts into a single incident
- Phishing analysis and root-cause identification
- Endpoint and process-level analysis
- Identification of obfuscation and hiding-in-plain-sight techniques
- Detection of network protocol abuse (DNS)
- Data exfiltration identification and analysis
- Incident escalation and impact-based decision-making
- Behavior-based threat analysis over signature-only detection
- Clear, structured SOC documentation

---

## 🔄 Investigation Challenges & Analyst Approach

The investigation required working through a large number of related alerts while maintaining a clear understanding of attacker progression.

To manage this, I:
- Anchored analysis around a single user and host
- Tracked process execution and file activity over time
- Correlated endpoint behavior with outbound network activity
- Focused on impact and behavior to guide escalation decisions

---

## 📈 Why This Project Matters

This lab reflects real SOC analyst responsibilities, including:
- Investigating confirmed compromises rather than blocked attempts
- Correlating alerts across multiple telemetry sources
- Making escalation decisions based on verified impact
- Communicating findings honestly and clearly for downstream response teams
