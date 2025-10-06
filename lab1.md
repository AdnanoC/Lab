# Splunk Lab 1: Incident Investigation & Brute Force Attack Analysis

## Lab Description

This lab simulates a real-world scenario where an attacker is brute-forcing login credentials against a web application’s admin portal. The goal is to use Splunk to identify the attack source, de[...]

> All sensitive data is anonymized for compliance.

---

## Investigation Steps

### 1. Identify Malicious Activity

**Splunk Query:**
```splunk
index="botsv1" sourcetype="stream:http" http_method=POST uri="/joomla/administrator/index.php"
```
- **Result:** 425 events detected  
[Click for screenshot](https://github.com/user-attachments/assets/6aae804c-85c6-4364-8873-2980389906a3)



---

### 2. Find the Source IP Address

- **Action:** Examined the `src_ip` field to determine which IP was responsible for most events.

![Source IP Breakdown](./screenshots/2.png)

- **Observation:**  
  - `23.22.63.114` = 412 events (96.941%)
  - `40.80.148.42` = 13 events (3.059%)

---

### 3. Filter by Top Source IP

**Splunk Query:**
```splunk
index="botsv1" sourcetype="stream:http" http_method=POST uri="/joomla/administrator/index.php" src_ip="23.22.63.114"
```
- **Result:** 412 events from the top IP  
![Filtered by Top Source IP](./screenshots/3.png)

---

### 4. Find the Destination IP

- **Action:** Checked the `dest_ip` (destination IP) field.

![Destination IP Breakdown](./screenshots/4.png)

- **Observation:**  
  - Destination IP: `192.168.250.70` (100% of events)

- **Raw event evidence:**  
![Destination IP in Raw Log](./screenshots/5.png)

---

### 5. Analyze Credential Attempts

#### a. Identify Username and Password Used by Attacker

- **Action:** Inspected the `form_data` field within the event to extract login attempts.

- **Example (from evidence):**
  - Username: `admin`
  - Passwords used in different attempts (see below)

**Evidence for `form_data` field:**  
- Username and password attempt (`baby`):  
![Username and Password in form_data](./screenshots/6.png)

---

#### b. Visualize All Credential Attempts

**Splunk Query:**
```splunk
index="botsv1" sourcetype="stream:http" http_method=POST uri="/joomla/administrator/index.php" src_ip="23.22.63.114" | table timestamp,form_data
```
- **Action:** Sorted by timestamp to identify the first attempted password.

![Table of Credential Attempts](./screenshots/7.png)

---

## Key Findings (with Evidence)

| Step | Question                                 | Evidence (Screenshot) | Finding                        |
|------|------------------------------------------|----------------------|--------------------------------|
| 1    | Total POST login events                  | 1                    | 425                            |
| 2    | Main attack source IP                    | 2                    | 23.22.63.114                   |
| 3    | Events from main source IP               | 3                    | 412                            |
| 4    | Destination (web server) IP              | 4, 5                 | 192.168.250.70                 |
| 5    | Username attempted                       | 5, 6                 | admin                          |
| 6    | Example password attempted (from event)  | 6                    | baby (see form_data value)     |
| 7    | Full list of attempted passwords, sorted | 7                    | See screenshot for attempts    |

---

## Professional Insights

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
3. Document your process—valuable for both technical assessment and interview discussions.

---

## Note

All sensitive data (IP addresses, usernames, passwords) in this documentation is anonymized or replaced with dummy values for compliance and public sharing.
