# Task 6 — Security Audit of Internee.pk's Website

**Internee.pk Cybersecurity Internship**
**Prepared by:** Zunaira Shahzad
**University:** University of Education, Lahore

## Objective

Identify vulnerabilities in a web application and assess its data protection, using penetration testing techniques (SQL injection, XSS, CSRF) and automated scanning.

## Scope

Testing was performed against **[OWASP Juice Shop](https://owasp-juice.shop)**, a deliberately vulnerable web application, used as the practice/staging target as recommended by the task guidelines in place of the live Internee.pk environment.

## Tools Used

- [OWASP ZAP](https://www.zaproxy.org/) 2.17.0 — automated vulnerability scanning
- Manual browser-based testing — SQL injection, XSS

## Methodology

1. Ran ZAP's Automated Scan (spider + active scan) against `http://localhost:3000`.
2. Manually tested the login page for SQL injection.
3. Manually tested the search bar for Cross-Site Scripting (XSS).
4. Reviewed authentication design for CSRF risk.
5. Reviewed ZAP's passive alerts for configuration weaknesses.

## Summary of Findings

| # | Finding | Risk | Location |
|---|---------|------|----------|
| 1 | SQL Injection (authentication bypass) | High | Login page |
| 2 | DOM-based Cross-Site Scripting (XSS) | High | Search bar |
| 3 | CSRF protection (JWT-based) | Informational | Account/session handling |
| 4 | Content Security Policy (CSP) header not set | Medium | Site-wide |
| 5 | Cross-Domain Misconfiguration (CORS) | Medium | /scripts.js |
| 6 | Timestamp Disclosure | Low | /styles.css |
| 7 | Modern Web Application (informational) | Informational | /sitemap.xml |

## Key Findings

### 1. SQL Injection — Authentication Bypass (High)
- **Payload:** `' OR 1=1--` in the login email field
- **Result:** Logged in as `admin@juice-sh.op` without a valid password
- **Fix:** Use parameterized queries / prepared statements

### 2. DOM-based XSS (High)
- **Payload:** `<iframe src="javascript:alert('XSS')">` in the search bar
- **Result:** JavaScript executed in the browser (alert box triggered)
- **Fix:** Sanitize and encode all user input before rendering; add a CSP header

Full details, evidence screenshots, and remediation steps are in [`report/Task6_Security_Audit_Report.pdf`](./report/Task6_Security_Audit_Report.pdf).

## Repository Structure

```
.
├── README.md
├── LICENSE
├── .gitignore
├── report/
│   └── Task6_Security_Audit_Report.pdf   # Full audit report
└── screenshots/
    ├── sqli_proof.png
    ├── xss_proof.png
    └── zap_alerts/
        ├── csp_finding.png
        ├── cors_finding.png
        ├── timestamp_finding.png
        └── modernapp_finding.png
```

## Disclaimer

All testing was performed against **OWASP Juice Shop**, a project intentionally built for security training and practice. No testing was performed against production systems without authorization. This repository is for educational purposes as part of an internship assignment.
