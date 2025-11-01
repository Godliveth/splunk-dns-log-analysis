# 🧪 Splunk Basics – DNS Log Analysis

Day 23 of #30DaysOfSOC Challenge

---
# 🧭 Summary

This project demonstrates how a SOC Analyst investigates DNS activity using Splunk and Zeek JSON-formatted logs. The goal is to analyze domain query behavior, identify active sources, and understand query type distributions that may reveal suspicious network activity.

---

# 🎯 Objective

In this lab, you will:

1. Learn how to ingest and analyze DNS logs in Splunk.

2. Identify the most queried domain names and the most active clients.

3. Examine DNS query types to detect potential anomalies.

4. Interpret results from a SOC perspective — linking DNS behavior to possible threats.

---

🖥️ Lab Setup

✅ Splunk: Installed and accessible

✅ Data Source: Zeek DNS log in JSON format

🌐 Log File: Sample Zeek DNS JSON dataset used (not included here due to size constraints)

[📥 Download DNS Log File](./dns_logs/dns.json)

    Place the DNS JSON log file in a directory accessible to Splunk for ingestion.

---

# ⚙️ Steps to Upload DNS Log into Splunk

1. Go to Splunk Web → Settings → Add Data

2. Choose Upload and select your dns.json file

3. Set the following:

   a. Source type: _json (or custom: dns)

   b. Index: Create one named dns_lab

   c. Click Next → Review → Submit

   d. Verify successful ingestion:

       index=dns_lab | head 5

---

# 🔍 SPL Queries Used in the Analysis

✅ 1. Identify the Most Frequently Queried Domain Names

    index=dns_lab sourcetype="_json"
    | stats count by query
    | sort -count

---

✅ 2. Find the Most Active Source IPs Generating DNS Traffic

    index=dns_lab sourcetype="_json"
    | stats count by "id.orig_h"
    | sort -count

✅ 3. Breakdown of DNS Query Types (A, AAAA, CNAME, PTR)

    index=dns_lab sourcetype="_json"
    | stats count by qtype

---

🧠 Detection Insights

| Observation                                                    | SOC Interpretation                                                                 |
| -------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **High number of queries to specific domains**                 | Could indicate **Command and Control (C2) beaconing** or **DNS tunneling**.        |
| **Single host generating unusually high DNS traffic**          | May suggest **malware callback behavior** or a **misconfigured internal service**. |
| **Large proportion of uncommon record types (e.g., PTR, TXT)** | Possible **network reconnaissance** or **data exfiltration** attempts.             |
| **Consistent query failure patterns (NXDOMAIN responses)**     | Might reflect **malware using randomized domain generation (DGA)**.                |

    These insights show how DNS activity can reveal potential threats when analyzed in Splunk — turning raw data into meaningful detection logic.

---

📸 Screenshots

Below are the captured analysis results from Splunk searches:

1. Top Queried Domains

2. Most Active User IPs

3. DNS Query Type Breakdown

All screenshots can be viewed in the 📁 screenshots/ folder.

[🔗 View Screenshot Folder](./screenshots/)

---

📂 Project Structure
splunk-dns-log-analysis/
│
├── dns_logs/
│   └── dns.json
│
├── screenshots/
│   ├── Breakdown of DNS query types (A, AAAA, CNAME, PTR).png
│   ├── The most active user IPs generating DNS traffic.png
│   └── The most frequently queried domain names.png
│
└── README.md

---

🪪 Author

Godliveth Madu
SOC ANALYST TRAINEE 
