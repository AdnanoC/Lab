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
![425 Events Query](https://user-images.githubusercontent.com/624760/214072948-38e5b7d3-245d-4d7f-b2c7-2b7c8de4d798.png)

---