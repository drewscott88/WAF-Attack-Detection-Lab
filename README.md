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
