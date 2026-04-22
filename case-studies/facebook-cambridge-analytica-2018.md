# Case Study: Facebook–Cambridge Analytica Scandal (2018)

**Type:** Third-party API data misuse / consent violation
**Records Affected:** ~87 million Facebook users
**Data Exposed:** Profile data, likes, friend networks, location, psychological attributes

---

## What Happened

The Cambridge Analytica incident is worth examining precisely because it breaks the typical mental model of a data breach. No system was hacked. No vulnerability was exploited. No attacker broke into a database. Everything that happened was technically permitted under Facebook's platform policies at the time — which is exactly the problem.

A personality quiz app called "This Is Your Digital Life" was published on the Facebook platform by a Cambridge University researcher. Approximately 270,000 users installed and used the app. Under Facebook's Graph API policies at the time, a user who granted an app access to their data also, by default, granted that app access to their friends' data — without those friends being notified or consenting.

This "friend permissioning" model meant that 270,000 installations translated to data collection on approximately 87 million people. That data — including profile information, page likes, location check-ins, and relationship networks — was transferred to Cambridge Analytica, a political consulting firm that used it to build detailed psychological profiles and develop targeted political messaging.

Allegedly this data influenced the 2016 U.S. presidential election and the Brexit referendum, though the degree of actual impact remains contested.

---

## The Consent Architecture Failure

The core failure here was in how consent was structured:

**User consent was used to override non-consenting third parties.** When a user installed the app and clicked "allow," they were authorizing data collection not just for themselves but for their entire friend network — people who had made no such decision. This is a fundamental violation of informed consent principles: you cannot consent on behalf of someone else.

**The terms of service were not meaningful disclosure.** Facebook's data policies technically disclosed this behavior in their developer documentation. But no reasonable person reading the app's permissions dialog would have understood they were authorizing data collection for hundreds of people they were connected to.

**Facebook had the ability to restrict this and chose not to.** The friend-data permissioning model was not a technical limitation — it was a deliberate design choice made to make Facebook's platform more attractive to third-party developers. The data access model was eventually restricted in 2015, but the restriction was not retroactively enforced, and apps that had already collected data were not required to delete it.

---

## The Impact

### Financial
- The FTC fined Facebook $5 billion in 2019 — the largest privacy penalty in U.S. history at the time
- Facebook also agreed to a new privacy oversight framework under the settlement

### Reputational
- The scandal triggered a significant #DeleteFacebook movement and accelerated user decline among younger demographics
- It became a defining moment in public consciousness around data privacy and platform accountability
- Mark Zuckerberg testified before Congress, and the hearings exposed how poorly understood Facebook's data practices were even among regulators

### Regulatory
- The scandal accelerated the passage of stricter privacy legislation globally
- It became a central case study in the debate over GDPR enforcement, CCPA development, and broader data governance reform

---

## What Should Have Been Done Differently

| Failure | Countermeasure |
|---|---|
| Friend-graph permissioning model | Restrict data access to the installing user only; require explicit consent from each individual whose data is accessed |
| No retroactive enforcement on data deletion | Require third-party apps to certify and demonstrate deletion of previously collected data when access policies change |
| Opaque consent mechanisms | Plain-language disclosure of exactly what data will be shared and with whom, at the time of authorization |
| No data use auditing | Periodic audits of how third-party apps are using the data they were granted access to |

---

## Key Takeaway for IAM Practice

The Cambridge Analytica case illustrates a class of identity risk that is often overlooked in traditional IAM frameworks: **authorization scope creep via third-party delegation**. When a user delegates access to a third-party application, the question isn't just whether that delegation is authenticated — it's whether the scope of access granted is appropriate, auditable, and revocable.

For IAM practitioners: OAuth and similar delegation frameworks are powerful tools, but they require disciplined scope design. Access tokens should be scoped to the minimum necessary data for the minimum necessary duration. Broad, indefinite access grants to third-party applications are an organizational liability, not just a privacy concern.

---

## References
- Federal Trade Commission. (2019). FTC imposes $5 billion penalty and sweeping new privacy restrictions on Facebook. https://www.ftc.gov/news-events/news/press-releases/2019/07/ftc-imposes-5-billion-penalty-sweeping-new-privacy-restrictions-facebook
- Anderson, R., & Moore, T. (2020). *The economics of information security.* Journal of Cybersecurity, 6(1).
