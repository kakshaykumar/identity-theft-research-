# Case Study: Equifax Data Breach (2017)

**Type:** Unpatched software vulnerability → unauthorized database access
**Records Affected:** ~147 million Americans
**Data Exposed:** Full names, SSNs, dates of birth, addresses, driver's license numbers, ~209,000 credit card numbers

---

## What Happened

The Equifax breach is, in many ways, the canonical example of what happens when patch management fails at scale. The entry point was a known, publicly disclosed vulnerability — CVE-2017-5638 — in Apache Struts, an open-source Java web application framework. The vulnerability was disclosed on March 7, 2017. A patch was available the same day. Equifax did not apply it.

Attackers began exploiting the vulnerability in mid-May 2017 — ten weeks after the patch was available. Over the following 76 days, they moved through Equifax's internal network, identified databases containing sensitive consumer data, and exfiltrated information through encrypted outbound channels. The intrusion was not detected until July 29, 2017.

The breach was publicly disclosed on September 7, 2017 — six weeks after internal discovery.

---

## Why It Escalated So Far

The initial vulnerability was bad, but several other failures compounded it:

**No network segmentation:** Once attackers breached the internet-facing application, they were able to reach internal databases without meaningful barriers. The web application server should not have had direct access to the consumer data systems.

**Lack of encryption at rest:** Much of the exposed data was stored unencrypted. Had Equifax encrypted its databases, the stolen data would have been unreadable even after exfiltration.

**Expired SSL certificate on monitoring tool:** The system Equifax used to inspect encrypted traffic had an expired certificate for 19 months, which meant 300+ internal network streams were not being inspected. The attackers used encrypted channels for their exfiltration — channels that went unmonitored.

**Data minimization failure:** Equifax was retaining Social Security numbers and other PII that it had no ongoing operational need to keep. Some of the exposed data had been collected years earlier for purposes that had long since concluded.

---

## The Impact

### Financial
- $700 million in regulatory settlements (including $425 million to affected consumers via the FTC)
- Total three-year cost exceeding $1.4 billion when remediation, legal fees, and ongoing proceedings are included
- Stock price declined more than 30% in the weeks following disclosure

### Legal and Regulatory
- Congressional hearings in both the House and Senate
- FTC, CFPB, and state attorney general investigations
- Multiple class-action lawsuits
- The CEO, CTO, and CSO all resigned within weeks of the disclosure

### Reputational
- For a company whose core business is assessing creditworthiness based on personal data, a breach of this scale created lasting questions about whether Equifax could be trusted as a steward of that data
- The company's own free credit monitoring service offered as remediation was widely mocked, as it redirected users to a separate website with its own terms of service

---

## What Should Have Been Done Differently

| Failure | Countermeasure |
|---|---|
| Unpatched vulnerability (CVE-2017-5638) | Mandatory patch SLA — critical severity patches applied within 48-72 hours |
| No network segmentation | Internet-facing applications should not have direct DB access; DMZ architecture |
| Unencrypted data at rest | AES-256 encryption for all PII databases |
| Monitoring system outage (expired certificate) | Certificate lifecycle management and automated expiry alerts |
| 76-day undetected dwell time | SIEM with behavioral baselines; active threat hunting |
| Excessive data retention | Data minimization policy — retain PII only as long as operationally required |

---

## Key Takeaway for IAM Practice

The Equifax breach was fundamentally a failure of access control and infrastructure hygiene, not a failure to stop a sophisticated attacker. The attackers used a well-known, published exploit. The systems they accessed were insufficiently isolated. The data they stole was unencrypted and unnecessarily retained.

For IAM practitioners specifically: this case underscores that identity security isn't just about authentication. It also includes controlling what authenticated users (and systems) can reach — and ensuring that the databases holding identity data are protected as aggressively as the systems used to access them.

---

## References
- U.S. Government Accountability Office. (2018). *Actions taken by Equifax and federal agencies in response to the 2017 breach (GAO-18-559).* https://www.gao.gov/products/gao-18-559
- IBM Security & Ponemon Institute. (2023). *Cost of a data breach report 2023.* https://www.ibm.com/reports/data-breach
