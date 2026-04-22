# Attack Vectors in Identity Theft

---

This document provides a detailed breakdown of the primary attack vectors used to facilitate identity theft, with notes on how each works, why it's effective, and what detection/prevention looks like in practice.

---

## Taxonomy Overview

```
Identity Theft Attack Vectors
│
├── Social Engineering
│   ├── Phishing (email, SMS, voice)
│   ├── Spear Phishing
│   ├── Business Email Compromise (BEC)
│   └── Pretexting
│
├── Technical Exploitation
│   ├── Software Vulnerability Exploitation
│   ├── Credential Stuffing
│   ├── Brute Force / Password Spraying
│   └── Man-in-the-Middle (MitM)
│
├── Account Takeover
│   ├── SIM Swapping
│   ├── Credential Reuse
│   └── Session Hijacking
│
└── Insider Threats
    ├── Malicious Insiders
    └── Negligent / Manipulated Insiders
```

---

## 1. Phishing and Social Engineering

### How It Works

Phishing exploits human psychology rather than technical vulnerabilities. A well-crafted phishing email mimics a trusted sender — a bank, an employer, a cloud service — and creates urgency that overrides the recipient's normal skepticism. The target clicks a link, enters credentials on a fake login page, and the attacker captures them in real time.

**Spear phishing** is phishing targeted at a specific individual, using personalized details to increase believability. An attacker who knows your name, employer, manager's name, and a recent company announcement can craft a message that's extremely difficult to distinguish from legitimate correspondence.

**Business Email Compromise (BEC)** is a spear phishing variant specifically targeting financial transactions — impersonating an executive to redirect a wire transfer, or impersonating a vendor to change banking details on an invoice. BEC losses globally exceed $50 billion since 2013 according to FBI data.

**Vishing (voice phishing)** uses phone calls — often with spoofed caller ID — to impersonate IT support, government agencies, or financial institutions and collect credentials or personal information verbally.

### Why It Works

- Creates urgency that bypasses rational skepticism
- Impersonates trusted brands with visual accuracy
- Exploits established workflows (e.g., "your account has been locked, click here to restore access")
- Often requires no technical sophistication — just convincing content

### Detection and Prevention

- Email gateway filtering (SPF, DKIM, DMARC records to validate sender authenticity)
- User training and regular phishing simulations
- Hardware security keys or app-based MFA (phishing-resistant — even captured credentials can't be replayed without the second factor)
- Strict callback verification procedures for financial transactions

---

## 2. Data Breaches via Software Vulnerabilities

### How It Works

Attackers identify unpatched or misconfigured software in an organization's internet-facing systems and exploit it to gain unauthorized access. The Equifax breach is the canonical example — a publicly disclosed vulnerability left unpatched for months. Other common vectors include:

- **SQL injection** — inserting malicious SQL into input fields to extract database contents
- **Remote code execution (RCE)** — exploiting vulnerabilities that allow attackers to run arbitrary code on a target system
- **API abuse** — exploiting poorly secured API endpoints to access or enumerate data without authorization

### Why It Works

- Large organizations with complex IT environments often have hundreds of applications to patch, and prioritization is imperfect
- Internet-facing systems are continuously scanned by automated tools looking for known CVEs
- The window between public disclosure of a vulnerability and mass exploitation can be as short as days

### Detection and Prevention

- Mandatory patch SLAs based on CVSS severity — critical vulnerabilities within 24-72 hours
- External attack surface management — continuous scanning of internet-facing assets
- Web Application Firewalls (WAF) as compensating control while patches are applied
- Vulnerability management program with clear ownership and tracking

---

## 3. Credential Stuffing

### How It Works

When a service is breached and user credentials are leaked, those username/password pairs are compiled and tested against other services — banking portals, email providers, corporate VPNs, SaaS applications. Because password reuse is extremely common, a single breach can cascade into account takeover across many unrelated services.

Tools like Sentry MBA and OpenBullet automate this testing at scale, checking thousands of credentials per minute and identifying valid combinations. The valid credentials are then sold, used for fraud, or retained for targeted attacks.

### Why It Works

- Password reuse is endemic despite years of security awareness messaging
- The credential databases feeding these attacks are enormous — hundreds of millions of leaked credentials are available on dark web markets
- Automation makes it economically viable to test every leaked credential against every major service

### Detection and Prevention

- Require unique passwords (enforced by integration with password manager recommendations)
- Implement MFA — even a successfully stuffed credential is useless without the second factor
- Rate limiting and CAPTCHA on login pages to slow automated attacks
- Monitor for unusual login patterns — multiple failed attempts from distributed IPs, or successful logins from unusual geographies

---

## 4. SIM Swapping

### How It Works

SIM swapping targets the SMS-based two-factor authentication that many services use as a secondary verification factor. The attacker contacts the victim's mobile carrier, impersonating the victim, and requests that the phone number be transferred to a new SIM card they control. Once successful, all SMS messages intended for the victim — including 2FA codes — are received by the attacker.

Attackers often combine SIM swapping with social engineering against carrier support staff, or leverage leaked personal information to pass identity verification questions.

### Why It Works

- Mobile carriers' identity verification procedures are often inadequate — frequently relying on knowledge-based authentication (last four of SSN, billing address) that is easily researched
- SMS-based 2FA is marketed as a security improvement but creates a dependency on mobile carrier security, which varies widely
- A successful SIM swap provides access to any account tied to that phone number

### Who Gets Targeted

High-value targets disproportionately: cryptocurrency holders, executives, social media accounts with valuable usernames, and individuals with large financial accounts linked to mobile numbers.

### Detection and Prevention

- Don't use SMS for 2FA where alternatives exist — use FIDO2 hardware keys or app-based authenticators
- Add a carrier account PIN or passphrase that must be provided before any account changes
- Monitor for sudden loss of mobile signal (a symptom of a completed SIM swap)
- Services should flag and delay account access when a number port or SIM change is detected

---

## 5. Insider Threats

### How It Works

Not all identity theft originates externally. Employees or contractors with legitimate access can:

- **Deliberately exfiltrate data** — motivated by financial gain, a competitor offer, or grievance
- **Accidentally expose data** — misconfigured sharing settings, sending data to the wrong recipient, falling for a social engineering attack targeting their legitimate credentials
- **Be manipulated** — social engineering that targets an employee's credentials or gets them to take an action that creates an opening (e.g., installing malware, clicking a phishing link, providing a password reset)

### Why It's Hard to Detect

Insider behavior looks like legitimate activity. An employee downloading a large file is doing their job — unless you know they downloaded 50,000 files the night before they resigned. The anomaly is intent and context, not the action itself.

### Detection and Prevention

- User and Entity Behavior Analytics (UEBA) to establish normal behavioral baselines and flag deviations
- Least privilege access — employees should only have access to data their role requires
- Offboarding procedures that immediately revoke access upon departure
- DLP (Data Loss Prevention) tools to monitor and restrict sensitive data movement
- Clear, well-communicated acceptable use policies combined with a culture where employees report unusual requests without fear of consequence

---

## Comparative Summary

| Vector | Technical Complexity | Scale | Primary Defense |
|---|---|---|---|
| Phishing | Low | High | MFA + training |
| Vulnerability exploitation | Medium-High | Variable | Patch management + WAF |
| Credential stuffing | Low (automated) | High | MFA + rate limiting |
| SIM swapping | Low-Medium | Targeted | App-based MFA + carrier PIN |
| Insider threats | Low | Targeted | UEBA + least privilege |

---

*See [`../countermeasures/iam-controls-reference.md`](../countermeasures/iam-controls-reference.md) for the full countermeasures breakdown.*
