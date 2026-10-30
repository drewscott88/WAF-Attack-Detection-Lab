# WAF Attack & Detection Lab

## Project Overview

Built a three-VM cybersecurity lab to simulate SQL injection attacks, analyze web application firewall detections, and implement layered security controls.

The project demonstrates how a vulnerable web application can be exploited, how ModSecurity with the OWASP Core Rule Set detects and blocks malicious requests, and why backend network isolation and secure coding are essential.

## Lab Architecture

```text
Kali Linux (Attacker)
192.168.60.30
        |
        v
Nginx + ModSecurity + OWASP CRS (WAF)
192.168.60.10
        |
        v
Apache + PHP + MariaDB + DVWA (Backend)
192.168.60.20
```

All systems were hosted in VirtualBox on an isolated internal lab network. The backend was later restricted using UFW to allow HTTP access only from the WAF.

## Technologies Used

- Kali Linux
- Ubuntu Linux
- VirtualBox
- Nginx reverse proxy
- ModSecurity v3
- OWASP Core Rule Set (CRS)
- Apache, PHP, MariaDB
- Damn Vulnerable Web Application (DVWA)
- UFW firewall
## SQL Injection Attack Simulation

### 1. Baseline: Successful SQL Injection

Configured DVWA at the Low security level and tested the SQL Injection module from Kali Linux against the backend web application.

**Test payload:**

```sql
1' OR '1'='1' #
```

The application returned all five DVWA user records instead of the single expected record, demonstrating a successful SQL injection against the vulnerable query.

### 2. WAF Detection Mode

Configured ModSecurity with OWASP CRS and set `SecRuleEngine DetectionOnly` to observe malicious requests without blocking them.

The same SQL injection payload continued to return five user records, but the WAF audit logs identified the request:

- **Rule 942100:** SQL Injection Attack Detected via libinjection
- **Matched variable:** `ARGS:id`
- **Inbound anomaly score:** 8
- **Rule 949110:** Inbound anomaly score exceeded threshold
- **HTTP response:** 200 (request allowed in DetectionOnly mode)

This demonstrated the distinction between detecting malicious activity and actively preventing it.

### 3. WAF Prevention Mode

Changed the ModSecurity configuration to `SecRuleEngine On`, validated the Nginx configuration, and reloaded the service.

Replayed the same SQL injection payload through the WAF. The request was blocked with **HTTP 403 Forbidden**.

The audit log confirmed the SQL injection detection and disruptive action, including rule `942100` and the anomaly-based blocking rule `949110`.

### Key Finding

A WAF in detection-only mode can generate useful security telemetry while still allowing an attack to succeed. Enabling blocking mode prevented the tested payload from reaching the vulnerable application through the protected route.
