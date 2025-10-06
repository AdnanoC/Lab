# Splunk Lab 1: Incident Investigation & Brute Force Attack Analysis

## 🔍 Overview
This lab investigates a simulated brute-force attack targeting a Joomla admin portal.  
Using Splunk, I identified the attacker’s source IP, analyzed credential attempts, and confirmed attack patterns.
---

## Investigation Steps

### 1. Identify Malicious Activity

**Splunk Query:**
```splunk
index="botsv1" sourcetype="stream:http" http_method=POST uri="/joomla/administrator/index.php"
```
<details>
  <summary>📸 Click for screenshot — Events 425</summary>
  <img width="624" height="147" alt="Events425" src="https://github.com/user-attachments/assets/6aae804c-85c6-4364-8873-2980389906a3" />
</details>



---

### 2. Find the Source IP Address

- **Action:** Examined the `src_ip` field to determine which IP was responsible for most events.

<details>
  <summary>📸 Click for screenshot — 412 Events (Main Source)</summary>
  <img width="624" height="123" alt="events412" src="https://github.com/user-attachments/assets/81c21865-b6bd-415f-9c9c-50139d1857e8" />
</details>

- **Observation:**  
  - `23.22.63.114` = 412 events (96.941%)
  - `40.80.148.42` = 13 events (3.059%)

---

### 3. Find the Destination IP

- **Action:** Checked the `dest_ip` (destination IP) field.

<details>
  <summary>📸 Click for screenshot — Destination IP</summary>
  <img width="583" height="227" alt="dest_ip" src="https://github.com/user-attachments/assets/f34a88e0-f81b-4146-9310-007a6ea4226d" />
</details>

- **Observation:**  
  - Destination IP: `192.168.250.70` (100% of events)

- **Raw event evidence:**  

<details>
  <summary>📸 Click for screenshot — Destination IP (Raw Evidence)</summary>
  <img width="624" height="410" alt="dest_ip2" src="https://github.com/user-attachments/assets/afa65f11-3d8d-42b0-b32c-24168e4b0f89" />
</details>

---

### 4. Analyze Credential Attempts

#### a. Identify Username and Password Used by Attacker

- **Action:** Inspected the `form_data` field within the event to extract login attempts.

- **Example (from evidence):**
  - Username: `admin`
  - Passwords used in different attempts (see below)

**Evidence for `form_data` field:**  
- Username and password attempt (`baby`):
    
<details>
  <summary>📸 Click for screenshot — Username and Password Attempt</summary>
  <img width="624" height="194" alt="user_pass" src="https://github.com/user-attachments/assets/ced11bfd-2039-4923-a565-0faa8a2ac326" />
</details>

---

#### b. Visualize All Credential Attempts

**Splunk Query:**
```splunk
index="botsv1" sourcetype="stream:http" http_method=POST uri="/joomla/administrator/index.php" src_ip="23.22.63.114" | table timestamp,form_data
```
- **Action:** Sorted by timestamp to identify the first attempted password.


<details>
  <summary>📸 Click for screenshot — Form Data (Time Sequence)</summary>
  <img width="624" height="343" alt="Time_form_data" src="https://github.com/user-attachments/assets/24e054d1-6983-48f0-9b73-20190ec732cc" />
</details>

---


##  Insights

- **Log Correlation:** Cross-referenced HTTP and system log types to confirm brute force activity and credential patterns.
- **Attack Attribution:** Identified the main attacking IP and methodically extracted credential attempts.
- **Remediation Steps:** Provided evidence for blocking the attacker’s IP and recommended reviewing authentication mechanisms for improved security.

---

## Screenshot Usage Notes

- All images are stored in the `/screenshots` folder.
- Reference images in markdown like this: `![Alt text](./screenshots/1.png)`
- Always redact or anonymize sensitive information before sharing.

---

## Next Steps

- Continue investigation for any signs of successful compromise.
- Document further steps and findings with additional screenshots as evidence.

---

## How to Use This Template

1. Duplicate this README.md for each new lab (e.g., `splunk-2/README.md`).
2. Update scenario details, queries, and anonymized data per lab.
3. Document your process—valuable for both technical assessment and interview discussions
 

---

## Note

All sensitive data (IP addresses, usernames, passwords) in this documentation is anonymized or replaced with dummy values for compliance and public sharing.
