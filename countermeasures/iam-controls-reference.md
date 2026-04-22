# IAM Controls Reference

---

A structured reference for the six countermeasure categories covered in the research paper, with implementation notes and relevant standards for each.

---

## 1. Identity and Access Management (IAM)

IAM governs who gets access to what, how that access is granted and revoked, and what they can do with it. A well-implemented IAM program is the foundation everything else builds on.

### Core Principles

**Least Privilege:** Every user, system, and service account should have exactly the access it needs to perform its function — and no more. Over-provisioned accounts are one of the most common factors that turn an initial compromise into a large-scale breach.

**Separation of Duties:** High-risk actions (large financial transactions, privileged system changes) should require more than one person to authorize, preventing any single compromised account from causing catastrophic harm.

**Zero Trust:** "Never trust, always verify." Rather than treating users inside the network perimeter as inherently trustworthy, zero trust requires continuous authentication and authorization for every access request, regardless of where it originates.

### Key Controls

| Control | What It Does | When to Use |
|---|---|---|
| RBAC (Role-Based Access Control) | Assigns access based on job role | Most standard enterprise environments |
| ABAC (Attribute-Based Access Control) | Assigns access based on contextual attributes (time, location, device posture) | High-sensitivity environments; conditional access policies |
| PAM (Privileged Access Management) | Monitors and controls administrator-level account use; often includes session recording | Any environment with admin accounts |
| SSO (Single Sign-On) | Centralizes authentication, reduces credential sprawl | Enterprises with many SaaS applications |
| Identity Governance | Automates access review, provisioning, and deprovisioning | Mid-to-large organizations; compliance-required environments |

### Standards and Frameworks
- **NIST SP 800-63** — Digital identity guidelines covering proofing, authentication, and federation
- **NIST SP 800-53** — Security and privacy controls for federal information systems (broadly applicable)
- **Zero Trust Architecture: NIST SP 800-207**

---

## 2. Multi-Factor Authentication (MFA)

MFA is consistently cited as one of the highest-return security controls available. Microsoft's internal research has found that MFA blocks over 99.9% of automated account attacks. It doesn't prevent every attack — but it eliminates the most common one: using a stolen password.

### The Three Factor Categories

| Category | Examples | Strength |
|---|---|---|
| Something you know | Password, PIN, security question | Weakest — can be stolen, guessed, or phished |
| Something you have | TOTP app, hardware key, smart card | Strong — requires physical possession |
| Something you are | Fingerprint, face, voice | Strong — but spoofable with deepfakes/injection attacks |

### MFA Methods Compared

| Method | Phishing-Resistant? | SIM-Swap Resistant? | Notes |
|---|---|---|---|
| SMS OTP | ❌ No | ❌ No | Deprecated as primary 2FA; SIM swap vulnerable |
| TOTP app (Authenticator) | ⚠️ Partial | ✅ Yes | Codes can be phished in real-time, but better than SMS |
| Push notification | ⚠️ Partial | ✅ Yes | Vulnerable to MFA fatigue attacks |
| FIDO2 / Hardware key (YubiKey) | ✅ Yes | ✅ Yes | Gold standard; cryptographically bound to domain |
| Passkeys | ✅ Yes | ✅ Yes | FIDO2-based, built into OS — growing adoption |

**Recommendation:** Transition away from SMS-based OTP where possible. App-based TOTP is a meaningful improvement. FIDO2 hardware keys or passkeys should be the target for high-value accounts (administrators, executives, finance).

### MFA Fatigue Attacks

Attackers aware that a target has MFA enabled will sometimes trigger repeated authentication prompts at odd hours, hoping the target approves one just to stop the notifications. Mitigations include number matching (the user must type a number shown on the screen, not just approve), and configuring apps to require context before approval.

---

## 3. Encryption and Tokenization

Encryption doesn't prevent breaches — but it determines whether a breach is a catastrophe or a manageable incident. Data stolen from an organization that encrypts properly is unreadable to the attacker.

### Encryption at Rest

**What it means:** Databases, file stores, and backups containing sensitive PII are encrypted so that physical or logical access to the storage medium alone is not sufficient to read the data.

**Algorithm:** AES-256 is the current standard for symmetric encryption at rest. It is computationally infeasible to brute-force with classical computing.

**Key management:** Encryption is only as strong as the key management around it. Encryption keys should be stored separately from the data they protect, with strict access controls and rotation policies.

**Where to apply it:**
- Production databases containing PII, credentials, or financial data
- Backup systems and archives
- Laptops and endpoint devices (full-disk encryption)
- Cloud storage buckets and object stores

### Encryption in Transit

**What it means:** Data moving between systems — between a browser and a server, between microservices, between data centers — is encrypted so it can't be intercepted and read.

**Standard:** TLS 1.3 (previous versions have known vulnerabilities and should be disabled).

**Common gaps:**
- Internal service-to-service traffic often travels unencrypted — "we're inside the network" is not a valid reason
- Certificate management failures (expired certs, self-signed certs accepted without validation)

### Tokenization

Tokenization replaces a sensitive value with a non-sensitive placeholder (a token) that has no mathematical relationship to the original. Unlike encryption, you can't reverse-engineer the original value from the token — you need to query the token vault that holds the mapping.

**Primary use case:** Payment card data. A tokenized PAN (primary account number) exposed in a breach is useless to the attacker. It also reduces the scope of PCI-DSS compliance audits — systems that only handle tokens, not actual card numbers, fall outside scope.

---

## 4. Security Awareness and Training

The most technically sophisticated security program can be undone by a single employee who clicks a phishing link or reads a password to someone on the phone. Security awareness training addresses the human attack surface.

### What Works

**Phishing simulations:** Regular, unannounced simulations that test employees' ability to identify phishing attempts. The goal is not to punish people who click — it's to measure the current click rate, identify training gaps, and give employees experience recognizing phishing in a low-stakes context.

**Role-specific training:** A finance employee needs to understand BEC attacks. A developer needs to understand credential management in code. A receptionist needs to understand pretexting and social engineering. Generic annual training serves no one well.

**Culture over compliance:** The best security training instills a mindset — "when something seems unusual, I verify before acting" — rather than just checking a compliance box. This requires organizational support: employees need to feel safe reporting suspicious activity and asking questions that might slow things down.

### What Doesn't Work

- Annual CBT modules that employees click through to completion
- Training that focuses exclusively on technical content rather than recognizing real-world scenarios
- Punitive responses to employees who report having been phished — this suppresses reporting

---

## 5. Monitoring and Anomaly Detection

The Equifax intrusion lasted 76 days undetected. T-Mobile's attacker described moving through the network without triggering meaningful alerts. In both cases, the damage was compounded not by the initial breach but by the extended window of undetected access.

### SIEM (Security Information and Event Management)

A SIEM aggregates log data from across the environment — firewalls, endpoints, servers, applications, cloud services — and correlates events in real time to identify patterns that suggest malicious activity. A well-tuned SIEM reduces alert fatigue by focusing on high-confidence indicators rather than generating noise.

**Key use cases for identity threat detection:**
- Multiple failed authentication attempts followed by a success
- Authentication from unusual geographies or devices
- Privilege escalation outside normal business hours
- Large data downloads or unusual API call volumes

### UEBA (User and Entity Behavior Analytics)

UEBA establishes what "normal" looks like for individual users and systems, then flags deviations. This is particularly valuable for detecting insider threats and compromised accounts that are behaving differently than the account owner normally would.

**Examples of anomalous behavior:**
- An employee accessing files they've never accessed before in large quantities
- A service account making API calls from a new IP address
- Login activity outside a user's normal hours or location

### SOAR (Security Orchestration, Automation and Response)

SOAR automates routine incident response actions — isolating a compromised account, blocking an IP, notifying a user of a suspicious login — so that the security team can focus on investigation rather than repetitive manual tasks. SOAR reduces mean time to respond (MTTR) and ensures consistent execution of response playbooks.

---

## 6. Compliance and Governance

### Key Frameworks

| Framework | Scope | Key Requirements |
|---|---|---|
| NIST SP 800-63 | Digital identity | Identity proofing, authentication assurance levels, federation |
| ISO/IEC 27001 | Information security management | Risk management, asset classification, control implementation, audit |
| GDPR | EU personal data | Consent, data minimization, breach notification within 72 hours, right to erasure |
| CCPA | California residents' data | Disclosure, opt-out rights, deletion requests, non-discrimination |
| HIPAA | Health information | PHI access controls, audit trails, encryption, breach notification |
| PCI-DSS | Payment card data | Network segmentation, encryption, access control, monitoring |

### Data Governance

Compliance frameworks only work when they're supported by active data governance — knowing what data you hold, where it lives, who has access to it, and how long you need to keep it. The most common compliance failure isn't a missing control — it's an organization that doesn't actually know what data it has and therefore can't protect it systematically.

A data governance program should include:
- Data inventory and classification (what is PII? what is regulated? what is internal only?)
- Data retention schedules with automated enforcement
- Access review cadences for sensitive data
- Data minimization practices — don't collect or retain data you don't have a clear use for

---

*Back to [`README.md`](../README.md)*
*See [`../research-notes/attack-vectors.md`](../research-notes/attack-vectors.md) for the attack vector breakdown.*
