# NETWORKWALKS-B083D-WK4-PROJECT-MEDIROZA-PEN-TESTING

# Mediroza General Hospital — Web Application Penetration Test

![Cybersecurity](https://img.shields.io/badge/Cybersecurity-Penetration%20Testing-red)
![Platform](https://img.shields.io/badge/Platform-Web%20Application-blue)
![Environment](https://img.shields.io/badge/Environment-Authorized%20Lab-green)
![Status](https://img.shields.io/badge/Project-Completed-success)

## Project Overview

The objective was to identify and demonstrate security weaknesses affecting a hospital web application and associated resources.

The assessment covered:

- Domain footprinting and reconnaissance
- Patient portal discovery
- Authentication testing
- SQL injection testing
- Authentication-bypass validation
- Access-control testing
- Downloading protected PDF reports
- Offline PDF password auditing
- PDF metadata analysis
- Legacy-directory discovery
- Database-backup exposure
- Sensitive-information exposure assessment

The exercise demonstrated how multiple weaknesses can be chained together to create significant security impact.

---

## Objectives

1. Footprint the target domain and identify accessible application functionality.
2. Locate the patient login portal.
3. Assess the authentication mechanism for common vulnerabilities.
4. Determine whether SQL injection could bypass authentication.
5. Assess whether authenticated access exposed other patients' documents.
6. Test the strength of password protection on downloaded PDF reports.
7. Examine document metadata for information that could reveal additional attack paths.
8. Investigate legacy directories discovered during the assessment.
9. Determine whether sensitive database backups were publicly accessible.
10. Document findings and provide remediation recommendations.

---

## Tools Used

| Tool | Purpose |
|---|---|
| Web Browser | Reconnaissance, application testing and validation |
| Kali Linux | Security testing environment |
| NetworkWalks Hash Calculator | PDF hash extraction/analysis |
| NetworkWalks Password Cracker | Authorized password-recovery exercise |
| John the Ripper | Offline password auditing |
| `rockyou.txt` | Dictionary for authorized password auditing |
| ExifTool | PDF metadata analysis |

---

# Methodology

```text
Reconnaissance
     ↓
Application Enumeration
     ↓
Authentication Testing
     ↓
SQL Injection Validation
     ↓
Access-Control Testing
     ↓
Document Acquisition
     ↓
Offline Password Auditing
     ↓
Metadata Analysis
     ↓
Legacy Directory Discovery
     ↓
Database Backup Exposure
     ↓
Impact Assessment
     ↓
Recommendations
```

---

# 1. Reconnaissance / Footprinting

The target domain was footprinted to identify publicly accessible application functionality.

The assessment looked for:

- Public pages
- Login portals
- Patient functionality
- Staff functionality
- Legacy directories
- Exposed files
- Potentially sensitive resources

A patient portal was identified during reconnaissance.

The portal provided patients with access to laboratory reports and stated that the reports were password protected.

---

# 2. Patient Portal Authentication Testing

The authentication functionality was tested for common web application vulnerabilities.

During testing, the login functionality was found to be susceptible to **SQL injection**, allowing the authentication mechanism to be bypassed in the authorized environment.

The issue indicated that user-controlled input was being incorporated into a database query without adequate parameterization.

### Security Impact

An authentication bypass is particularly serious for a healthcare application because the affected functionality protects access to medical information.

Potential impact included:

- Authentication bypass
- Unauthorized patient-portal access
- Access to records belonging to other users
- Download of protected medical documents

> The exact authentication-bypass payload is intentionally omitted from this public README.

---

# 3. Patient Data Access

After validating the authentication weakness, access to the patient portal was obtained.

The portal exposed laboratory reports belonging to multiple patients.

Three encrypted PDF laboratory reports were downloaded for the authorized assessment:

```text
patient_report_1.pdf
patient_report_2.pdf
patient_report_3.pdf
```

### Security Impact

The ability to access multiple patient documents demonstrated a serious confidentiality and access-control issue.

Depending on the contents of the reports, exposed information could include:

- Patient names
- Patient identifiers
- Dates of birth
- Laboratory results
- Referring physicians
- Laboratory reference numbers
- Other medical information

No patient information is reproduced in this repository.

---

# 4. PDF Password Security Assessment

The three downloaded PDFs were subjected to an offline password-strength assessment.

| File | Password Protection | Result |
|---|---|---|
| `patient_report_1.pdf` | Enabled | Password recovered quickly |
| `patient_report_2.pdf` | Enabled | Password recovered quickly |
| `patient_report_3.pdf` | Enabled | Password recovered after a longer dictionary-based attack |

The first two documents used extremely common passwords, demonstrating the risk of weak document-password practices.

The third document took longer to recover but was successfully cracked using the NetworkWalks Password Cracker and an offline dictionary-based approach with `rockyou.txt`.

Actual recovered passwords are intentionally not published.

---

# 5. John the Ripper

John the Ripper was used from Kali Linux to perform offline password auditing against extracted PDF hashes.

The general workflow was:

```text
Protected PDF
     ↓
PDF hash extraction
     ↓
John the Ripper
     ↓
Dictionary-based password audit
     ↓
Recovered password
     ↓
PDF decryption/validation
```

A representative hash-extraction workflow was:

```bash
./john/run/pdf2john.pl /path/to/protected.pdf > hash.txt
```

The extracted hash was then supplied to John using the PDF format and an appropriate wordlist:

```bash
./john/run/john --format=PDF --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

The recovered password was validated by opening the corresponding PDF.

---

# 6. NetworkWalks Password Cracker

The NetworkWalks Academy Password Cracker was also used.

The workflow consisted of:

1. Extracting the PDF hash.
2. Supplying the hash to the password cracker.
3. Selecting an appropriate wordlist.
4. Running the password-auditing process.
5. Recovering the password.
6. Validating the password by opening the PDF.

The tool successfully recovered the password for the third PDF.

This demonstrated the practical difference between very common passwords and passwords that require a larger dictionary search.

---

# 7. PDF Metadata Analysis

After recovering access to the PDF documents, ExifTool was used to inspect their metadata.

Example:

```bash
exiftool patient_report_3.pdf
```

The metadata contained information that was significant from a security perspective.

One document contained a comment indicating that a **database backup had been moved to an `/old` directory and should not be deleted**.

This represented an important information-disclosure finding because document metadata exposed information about the underlying web application's file structure.

---

# 8. Legacy Directory Discovery

Following the metadata lead, the `/old/` directory was investigated within the authorized scope.

The directory was publicly accessible and exposed a database backup file.

### Security Impact

A publicly accessible database backup can potentially expose:

- Application data
- User information
- Staff information
- Internal records
- Historical data
- Credentials or credential-related information
- Other sensitive application content

---

# 9. Database Backup Exposure

The exposed SQL database backup was inspected in the authorized environment.

The backup contained tables and records associated with hospital operations.

The exposed data included categories such as:

- Staff records
- Staff contact information
- Employment information
- Salary-related information
- National identification information
- Shareholder records
- Shareholding information
- Other internal application data

Exact personal records are intentionally excluded from this repository.

---

# Attack Chain

```text
Publicly accessible website
          ↓
Patient portal discovered
          ↓
SQL injection identified
          ↓
Authentication bypass
          ↓
Patient portal access
          ↓
Multiple protected medical PDFs downloaded
          ↓
Weak PDF passwords recovered
          ↓
PDF contents accessed
          ↓
PDF metadata inspected
          ↓
Metadata revealed legacy backup location
          ↓
Public /old/ directory discovered
          ↓
Database backup accessed
          ↓
Sensitive staff/shareholder information exposed
```

This illustrates how a relatively simple application vulnerability can become significantly more serious when combined with weak document passwords, information disclosure, poor access controls and insecure backup storage.

---

# Key Findings

## Finding 1 — SQL Injection in Patient Authentication

**Severity: Critical**

The patient authentication mechanism was susceptible to SQL injection, allowing authentication controls to be bypassed.

### Impact

Potential unauthorized access to protected patient functionality and medical documents.

### Recommendation

- Use parameterized queries/prepared statements.
- Never concatenate user input directly into SQL statements.
- Use a secure ORM/database abstraction layer.
- Apply server-side input validation.
- Use least-privilege database accounts.
- Disable verbose SQL errors in production.
- Perform regular SQL-injection testing.

---

## Finding 2 — Broken Access Control / Patient Data Exposure

**Severity: Critical**

After bypassing authentication, multiple patient reports could be accessed and downloaded.

### Impact

Potential disclosure of confidential medical information.

### Recommendation

Implement server-side authorization checks for every patient resource.

A patient should only be able to retrieve records belonging to their authorized account.

Authorization should never rely solely on:

- Hidden URLs
- Client-side checks
- Filename obscurity
- UI restrictions

---

## Finding 3 — Weak PDF Passwords

**Severity: High**

Sensitive PDF documents were protected using passwords that could be recovered through dictionary-based password auditing.

### Impact

Anyone obtaining the encrypted files could potentially recover the passwords and access the documents.

### Recommendation

- Use strong, randomly generated passwords.
- Use sufficiently long passphrases.
- Avoid common passwords.
- Avoid password reuse.
- Consider authenticated document delivery instead of relying on PDF passwords as the primary security boundary.

---

## Finding 4 — Sensitive Information Disclosure Through PDF Metadata

**Severity: High**

PDF metadata exposed information about a legacy database-backup location.

### Impact

Metadata can reveal:

- Server directory structures
- Internal file locations
- Application architecture
- Backup locations
- Development or migration details

### Recommendation

- Sanitize metadata before distributing documents.
- Remove unnecessary metadata during PDF generation.
- Avoid placing internal operational notes in user-accessible documents.
- Review document-generation templates.

---

## Finding 5 — Publicly Accessible Database Backup

**Severity: Critical**

A database backup was accessible through a publicly reachable legacy directory.

### Impact

A database backup may contain extensive sensitive information and can bypass application-level security controls entirely.

### Recommendation

- Store backups outside the public web root.
- Disable directory listing.
- Deny direct access to backup extensions.
- Restrict backup-file permissions.
- Encrypt sensitive backups.
- Use secure off-site backup storage.
- Regularly scan web roots for exposed backups.
- Remove obsolete `/old`, `/backup`, `/tmp` and `/test` directories.
- Prevent public access to `.sql`, `.bak`, `.zip` and similar backup files.

---

# Overall Security Recommendations

## Application Security

- Use prepared statements/parameterized queries.
- Implement centralized input validation.
- Implement secure authentication.
- Apply server-side authorization checks.
- Prevent verbose production errors.
- Use secure session management.
- Apply least privilege.

## Data Protection

- Encrypt sensitive data at rest where appropriate.
- Protect medical documents with strong access controls.
- Avoid weak document passwords.
- Minimize sensitive information in downloadable documents.
- Apply data-retention policies.

## File and Backup Security

- Keep backups outside the web root.
- Restrict filesystem permissions.
- Disable directory indexing.
- Remove obsolete application directories.
- Block direct access to backup extensions.
- Regularly scan for accidentally exposed files.

## Metadata Security

- Remove unnecessary PDF metadata.
- Review document-generation workflows.
- Avoid internal comments in externally distributed documents.
- Perform metadata checks before publishing sensitive documents.

## Monitoring

Implement:

- Web-application logging
- Authentication monitoring
- SQL-injection detection
- File-access monitoring
- Alerts for unusual document downloads
- Backup-access monitoring
- Regular vulnerability assessments

---

# Skills Demonstrated

This project provided practical experience with:

- Web reconnaissance
- Attack-surface enumeration
- Authentication testing
- SQL injection identification
- Authentication-bypass validation
- Access-control testing
- Sensitive-data exposure assessment
- PDF security assessment
- Password auditing
- John the Ripper
- Wordlist-based password recovery
- ExifTool
- Metadata analysis
- Directory enumeration
- Backup-file exposure assessment
- Impact analysis
- Penetration-testing methodology
- Responsible disclosure

---

# Lessons Learned

### 1. Vulnerabilities can be chained

The SQL injection issue was only the beginning. It provided access to functionality that exposed additional resources.

### 2. Encryption does not compensate for weak passwords

A password-protected document is only as strong as the password protecting it.

### 3. Metadata matters

Metadata can reveal information that is not visible in the main document content.

### 4. Backup files are high-value targets

A forgotten database backup can expose significantly more information than the original application.

### 5. Legacy directories require security review

Directories such as `/old/` can become forgotten attack surfaces after migrations or application changes.

### 6. Follow the evidence

The assessment progressed from reconnaissance to authentication testing, then from document analysis to metadata discovery and finally to backup exposure.

---

# Suggested Repository Structure

```text
mediroza-hospital-pentest/
│
├── README.md
└── penetration-testing-report_protected.pdf
```

---

# Project Evidence

The assessment was validated through screenshots and terminal output showing:

- Patient portal discovery
- Authentication testing
- Downloaded encrypted PDF reports
- NetworkWalks Password Cracker results
- John the Ripper output
- Successful PDF password recovery
- ExifTool metadata inspection
- Discovery of the legacy `/old/` directory
- Discovery of the exposed database backup

Any screenshots published publicly should be redacted first.

---

# Internship Context

**Program:** NetworkWalks Cybersecurity Internship  
**Project:** Week 4 – Mediroza Penetration Testing / PDF Password Auditing  
**Environment:** Authorized educational/CTF-style target  
**Primary Platform:** Kali Linux

---

# Disclaimer

This repository documents cybersecurity techniques performed within an authorized training environment.

The information is provided for educational purposes and responsible security research. Do not perform SQL injection, password auditing, unauthorized document access, directory enumeration or database-access testing against systems without explicit permission.

---

## Author: Patrick Ekata Alabi

**Cybersecurity Intern**  
NetworkWalks Cybersecurity Internship – Week 4
      ▼
Sensitive Staff & Shareholder Data Exposure
