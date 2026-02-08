# 🛡️ SOC Alert Triage – Multi-Stage Phishing to Data Exfiltration (SOC Simulator)

## 📌 Overview

This project documents a simulated **SOC alert triage and incident investigation** completed in a SOC Simulator environment using Splunk-style logs, endpoint telemetry, and network activity.

The scenario mirrors a realistic SOC investigation involving:
- A successful phishing attack
- Multiple alerts tied to the same user and host
- Correlation across endpoint, network, and file activity
- Identification of post-compromise behavior
- Escalation based on confirmed impact (data exfiltration)

Although alerts were generated individually, analysis was conducted at the **incident level**, reflecting real-world SOC workflows.

---

## 🔍 Environment & Tooling

- **SIEM:** Splunk-style log interface  
- **Log Sources:**
  - Email telemetry
  - Endpoint process execution logs
  - File system activity
  - Network / DNS activity
- **Scenario Type:** Phishing → Post-Exploitation → Data Exfiltration

---

## 🧠 Investigation Summary

During the simulation, I investigated **17 alerts** generated over a short time window. While each alert was scored individually by the platform, all alerts were tied to a **single attack chain** impacting one high-value user.

The investigation revealed:
- Initial compromise via phishing attachment
- Execution of PowerShell-based tooling
- Unauthorized access to sensitive internal file shares
- Active outbound communication and data exfiltration

Understanding the full scope required correlating alerts rather than treating them as isolated events.

---

## 📂 Case Breakdown (High-Level)

### 🟢 Initial Access – Phishing

**Indicators Observed:**
- Phishing email sent from external domain (`john@hatmakereurope.xyz`)
- Malicious attachment: `ImportantInvoice-Febrary.zip`
- High-value target (CEO)

**Action Taken:**  
- Identified phishing email as the root cause  
- Correlated downstream alerts back to initial attachment execution  

**Outcome:**  
Confirmed initial access vector

---

### 🟡 Post-Compromise Activity

**Indicators Observed:**
- PowerShell execution (`powershell.exe`)
- Download and execution of `PowerView.ps1`
- Use of legitimate Windows binaries (`rdpclip.exe`, `net.exe`, `robocopy.exe`)
- Mapping of internal file share (`\\FILESRV-01\\SSF-FinancialRecords`)

**Action Taken:**
- Correlated process execution and file access activity  
- Identified data staging behavior prior to exfiltration  

**Outcome:**  
Confirmed unauthorized internal access and preparation for data theft

---

### 🔴 True Positive – Escalated Incident (Data Exfiltration)

**Key Indicators:**
- DNS / outbound communication using `nslookup.exe`
- PowerShell pipelines establishing external connections
- Use of `powercat.ps1` for tunneling
- External destination: `2.tcp.ngrok.io:19282`

**Why This Required Escalation:**
- Confirmed compromise of endpoint
- Unauthorized access to sensitive financial data
- Verified outbound data transfer to external infrastructure

**Action Taken:**
- Correlated endpoint, file, and network activity  
- Identified affected user, host, and destination infrastructure  
- Classified activity as active data exfiltration  
- Escalated for immediate containment and response  

**Outcome:**  
Incident escalated due to confirmed impact

---

## 🧩 Correlation & Analysis Highlights

- Correlated **17 alerts** into a single incident narrative
- Mapped progression across attack stages:
  - Phishing → Execution → Discovery → Staging → Exfiltration
- Distinguished between:
  - Suspicious activity
  - Confirmed malicious behavior
- Used behavior and impact to drive decisions, not alert volume

---

## 📉 Lessons Learned

### 1. Alert Volume vs Incident Scope
Multiple alerts do not always represent multiple incidents.

**Takeaway:**  
Incident-level correlation is essential for accurate severity assessment.

---

### 2. Tool Familiarity vs Behavioral Analysis
Some observed commands and tooling were unfamiliar at first glance.

**Takeaway:**  
Understanding attacker *intent* through behavior can be sufficient to make correct escalation decisions, even without perfect command-level familiarity.

---

### 3. Visibility Gaps Matter
Indicators of privilege escalation or lateral movement were suspected but not fully visible in the provided logs.

**Takeaway:**  
Documenting visibility gaps is critical for guiding future detection improvements.

---

## 🧪 Skills Demonstrated

- SOC alert triage and prioritization
- Multi-alert correlation into a single incident
- Phishing analysis and root-cause identification
- Endpoint and process analysis
- Identification of living-off-the-land techniques
- Data exfiltration detection
- Incident escalation decision-making
- Clear, structured SOC documentation

---

## 🔄 Investigation Challenges & Analyst Approach

The investigation required working through a large number of related alerts while maintaining a clear understanding of attacker progression.

To manage this, I:
- Anchored analysis around a single user and host
- Tracked process execution and file activity over time
- Correlated endpoint behavior with outbound network activity
- Focused on impact to guide escalation decisions

---

## 📈 Why This Project Matters

This lab reflects real SOC analyst responsibilities, including:
- Investigating confirmed compromises rather than blocked attempts
- Correlating alerts across multiple telemetry sources
- Making escalation decisions based on verified impact
- Communicating findings clearly for downstream response teams

It emphasizes **incident-level thinking, correlation, and judgment** over tool-specific syntax.
