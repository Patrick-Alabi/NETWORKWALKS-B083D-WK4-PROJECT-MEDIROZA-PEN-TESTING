# NETWORKWALKS-B083D-WK4-PROJECT-MEDIROZA-PEN-TESTING

# Mediroza General Hospital — Web Application Penetration Test

![Cybersecurity](https://img.shields.io/badge/Cybersecurity-Penetration%20Testing-red)
![Platform](https://img.shields.io/badge/Platform-Web%20Application-blue)
![Environment](https://img.shields.io/badge/Environment-Authorized%20Lab-green)
![Status](https://img.shields.io/badge/Project-Completed-success)

## Overview

This project was an authorized black-box penetration-testing exercise conducted against the Mediroza General Hospital web application as part of my cybersecurity training with NetworkWalks.

The objective was to identify vulnerabilities in the hospital's web application, demonstrate their potential impact, and document appropriate remediation measures.

> **Important:** This project was conducted within an authorized educational environment. No unauthorized systems were targeted.

---

## Objectives

The assessment focused on:

- Performing reconnaissance and footprinting
- Identifying exposed application entry points
- Assessing authentication mechanisms
- Testing user-input handling
- Identifying SQL injection vulnerabilities
- Demonstrating authentication bypass
- Assessing access controls around patient laboratory reports
- Testing the security of password-protected PDF documents
- Examining PDF metadata
- Investigating exposed server resources
- Assessing the impact of sensitive information disclosure
- Providing remediation recommendations

---

## Attack Chain

The assessment demonstrated the following attack path:

```text
Reconnaissance
      │
      ▼
Patient Portal Discovery
      │
      ▼
Authentication Testing
      │
      ▼
SQL Injection
      │
      ▼
Authentication Bypass
      │
      ▼
Access to Patient Reports
      │
      ▼
Password-Protected PDF Analysis
      │
      ▼
Password Recovery
      │
      ▼
PDF Metadata Analysis
      │
      ▼
Discovery of Exposed Backup Location
      │
      ▼
Database Backup Exposure
      │
      ▼
Sensitive Staff & Shareholder Data Exposure
