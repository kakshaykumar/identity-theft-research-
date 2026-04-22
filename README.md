# Identity Theft in the Digital Age

---

## Overview

This research paper examines identity theft through three lenses: what has actually happened (real-world case studies from 2017–2023), how it happens (common attack vectors), and what organizations can realistically do about it (countermeasures grounded in current IAM frameworks).

The central argument is that large-scale identity theft is rarely the result of novel, exotic attacks. More often, it's the result of known vulnerabilities left unpatched, access controls set too permissively, data held longer than necessary, and monitoring too shallow to catch intrusions before the damage is done. The adversaries are real and capable — but the opportunities they exploit are largely created by organizations themselves.

This is a literature-based research and analysis paper written for a graduate-level Identity and Access Management course. The focus is on connecting real incident case studies to the technical and organizational failure modes that enabled them, and deriving actionable lessons from each.

---

## Repository Structure

```
identity-theft-research/
│
├── README.md                                    ← You are here
│
├── research-docs/
│   └── identity-theft-research-paper.pdf        ← Full formatted research paper
│
├── case-studies/
│   ├── equifax-2017.md                          ← Equifax breach deep dive
│   ├── facebook-cambridge-analytica-2018.md     ← Cambridge Analytica scandal analysis
│   └── t-mobile-2021.md                         ← T-Mobile breach analysis
│
├── research-notes/
│   ├── attack-vectors.md                        ← Attack vector taxonomy and analysis
│   └── business-impact-framework.md             ← How identity theft affects organizations
│
├── countermeasures/
│   └── iam-controls-reference.md               ← IAM controls, MFA, encryption, monitoring
│
├── diagrams/
│   └── identity-theft-lifecycle.md             ← Lifecycle diagram + attack surface map
│
└── notes/
    └── references.md                           ← Key sources with summaries
```

---

## Why This Topic Matters

In 2023 alone, the FTC received 1.1 million identity theft reports in the United States. The cost of a data breach globally averaged $4.45 million per incident (IBM, 2023). Beyond the numbers, identity theft has become a structural problem in digital infrastructure — one that affects individuals, organizations, and increasingly, national security systems.

What's striking when you look at the major incidents is how preventable many of them were. The Equifax breach came down to an unpatched server. The Cambridge Analytica scandal exploited a permissive API policy that Facebook had the ability — and arguably the obligation — to restrict. T-Mobile's breach involved lateral movement so easy the attacker described T-Mobile's security posture as "awful" in public interviews. These weren't sophisticated zero-day attacks. They were the consequences of organizational decisions about how much to invest in identity security.

---

## Case Studies Covered

| Incident | Year | Type | Records Affected | Key Failure |
|---|---|---|---|---|
| Equifax Breach | 2017 | Unpatched vulnerability | ~147 million | CVE-2017-5638 left unpatched for months |
| Facebook–Cambridge Analytica | 2018 | API data misuse | ~87 million | Overly permissive third-party data access |
| T-Mobile Breach | 2021 | Network intrusion + lateral movement | ~76 million | No segmentation, exposed internal systems |

Each case study in [`case-studies/`](case-studies/) breaks down the incident, its impact, and the specific lessons that apply to identity security practice.

---

## Attack Vectors Covered

- Phishing and Business Email Compromise (BEC)
- Data breaches via software vulnerabilities and credential stuffing
- Insider threats (both malicious and accidental)
- SIM swapping against SMS-based 2FA
- Credential reuse across services

See [`research-notes/attack-vectors.md`](research-notes/attack-vectors.md) for the full breakdown.

---

## Countermeasures Framework

The countermeasures section is organized around six control categories:

1. **Identity and Access Management (IAM)** — RBAC, ABAC, SSO, PAM, Zero Trust
2. **Multi-Factor Authentication** — factor types, FIDO2, why SMS 2FA is insufficient
3. **Encryption and Tokenization** — at-rest, in-transit, tokenization for PCI scope reduction
4. **Security Awareness and Training** — phishing simulations, culture change
5. **Monitoring and Anomaly Detection** — SIEM, UEBA, SOAR
6. **Compliance and Governance** — NIST SP 800-63, ISO/IEC 27001, GDPR, CCPA

See [`countermeasures/iam-controls-reference.md`](countermeasures/iam-controls-reference.md) for detailed notes on each.

---

## Emerging Threats Covered

- Synthetic identity fraud (AI-constructed identities using real SSN fragments)
- Deepfake-based biometric spoofing
- Cloud IAM misconfiguration (over-permissioned roles, multi-cloud visibility gaps)
- Quantum computing threats to PKI and current encryption standards
- AI-powered social engineering and automated credential attacks

---

## Skills and Concepts Demonstrated

- Identity lifecycle analysis and IAM framework design principles
- Case study methodology — connecting incidents to root causes to lessons
- Attack vector taxonomy (MITRE ATT&CK-aligned threat categorization)
- Defense-in-depth thinking applied to identity and access management
- Regulatory framework literacy (NIST, GDPR, CCPA, ISO 27001)
- Understanding of emerging identity threats (synthetic fraud, deepfakes, PQC)
- Business impact framing for cybersecurity decisions

---

## References

[`notes/references.md`](notes/references.md).

Selected key sources:

- Grassi, P. A., et al. (2017). Digital Identity Guidelines — NIST SP 800-63-3.
- IBM Security & Ponemon Institute. (2023). Cost of a Data Breach Report 2023.
- Verizon. (2023). Data Breach Investigations Report.
- U.S. GAO. (2018). Actions Taken by Equifax and Federal Agencies — GAO-18-559.
- Anderson, R., & Moore, T. (2020). The Economics of Information Security.

---
