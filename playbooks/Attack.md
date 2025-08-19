# SOC Playbook: Attack Investigation

## 1. Objective
Identify, investigate, and respond to potential attacks detected by SIEM alerts, ensuring timely mitigation and accurate classification of incidents.

---

## 2. Detection
- Alerts triggered in SIEM (e.g., SQL injection, command injection, brute-force login, path traversal).  
- Indicators include suspicious keywords, abnormal file access attempts, repeated login failures, or payload-like patterns.  

---

## 3. Analysis
### Step 1 – Validate Alerts
- Check if the request contains:  
  - **Accessing what should not be accessed** (e.g., `/etc/passwd`).  
  - **Containing what should not appear** (e.g., SQL commands, encoded payloads).  

### Step 2 – Identify False Positives
- Internal SQL queries misclassified as SQL injection.  
- Benign file uploads misclassified as malware.  
- VPN logins from outsourced teams.  
- Internal commands flagged by keyword rules.  

### Step 3 – Confirm True Positives
- **SQL injection** → look for SQL errors or DB response.  
- **Command injection** → identify obfuscated commands (e.g., `invoke`, base64, IP addresses).  
- **Brute-force login** → repeated failed attempts from same IP.  
- **Path traversal** → abnormal file access with `../`.  

If unclear, export **pcap** for full traffic analysis (data leakage, payload execution, file transfer).  

---

## 4. Response
- **False positive** → document and close ticket.  
- **True attack, unsuccessful** → block source IP and close incident.  
- **True attack, successful** →  
  - Escalate to Incident Response (IR) team.  
  - Create incident report (timeline, IoCs, affected systems).  
  - Notify stakeholders.  

---

## 5. Prioritization
- Investigate **high-severity alerts** first (command injection, file upload).  
- Prioritize alerts with **high-frequency IPs**.  

---

## 6. Example Cases
- **SQL Injection Attempt**:  
  - Detected `UNION SELECT` in request.  
  - Response showed no DB reply → blocked IP → closed as unsuccessful.  

- **Brute Force Login**:  
  - Multiple weak password attempts from one IP.  
  - Action → blocked IP, generated ticket.  

- **Path Traversal**:  
  - Request contained `../` to restricted directories.  
  - Attempt failed → blocked IP, documented case.  
