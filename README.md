# OAuth 2.1 Government Content Access Control (GCAC)

## Background and Motivation

Governments increasingly argue that age and eligibility verification must be outsourced to private companies because deploying public infrastructure is “too complex” or “too risky.” At the same time, many of the proposed private-sector solutions rely on highly sensitive data collection such as facial scans, biometrics, and identity documents.

This creates a contradiction: systems justified as *protecting children and privacy* often require intrusive data collection by commercial entities whose incentives include data retention, model training, and secondary data use.

The **Australian Government’s own response** to its Age Verification Roadmap acknowledges that the private age-assurance market is still immature:

> *“The Roadmap notes action already underway by industry to introduce and improve age assurance and finds that the market for age assurance products is immature, but developing.”*  
> *…*  
> *“Age assurance technologies cannot yet meet all these requirements. While industry is taking steps to further develop these technologies, the Roadmap finds that the age assurance market is, at this time, immature.”*  
> — [Australian Government response to the Roadmap for Age Verification][12]

Relying on immature, privacy-invasive commercial systems is a significant risk. GCAC proposes a **standards-based, privacy-preserving public alternative**.

This proposal is not tied to any specific national policy agenda. It is intended as a technical standard that governments *could* adopt to meet regulatory goals **without enabling mass surveillance or commercial data harvesting**.

---

# Policy Context

## 🇬🇧 United Kingdom — Online Safety Act

The UK requires platforms to implement age verification for adult or otherwise harmful content. Acceptable methods include facial analysis, photo ID, or credit card checks. ([gov.uk][1])

Although the law discourages unnecessary data retention, the permitted verification methods inherently involve sensitive personal data. This has raised concerns about:

- Privacy risks  
- Data breaches  
- Inconsistent enforcement  
- Easy circumvention using VPNs ([TechRadar][2])

---

## 🇦🇺 Australia — Under-16 Social Media Restrictions

Australia has passed legislation restricting social media access for users under 16. Platforms must take “reasonable steps” to prevent underage access. ([eSafety Commissioner][3])

Non-compliance can result in substantial fines. ([Reuters][4]) However:

- Enforcement expectations remain unclear  
- Methods may involve intrusive data collection  
- Effectiveness and proportionality are widely debated ([Infrastructure Submission][5])

---

## 🇪🇺 European Union — DSA & GDPR

The EU’s **Digital Services Act (DSA)** and **GDPR** both impose obligations to protect minors. However:

- GDPR may require parental consent or age checks  
- DSA requires risk mitigation and child safety measures  
- **No standardized age-verification framework exists**

This results in fragmented approaches, often relying on third-party identity sharing. ([SafePaper][6])

---

## 🇪🇺 Proposed “Chat Control” Legislation

Some EU proposals would require large-scale scanning of private communications for child safety enforcement. ([FightChatControl][7])

GCAC differs fundamentally: it does **not** require access to private messages. Instead, it only provides proof of legal eligibility to access regulated services.

---

# Problems With Current Approaches

## ❌ 1. Privacy Invasion

Many current systems require users to submit:

- Facial scans  
- Government IDs  
- Credit card details  

to private platforms or third-party vendors. ([gov.uk][1])

Risks include:

- Data breaches  
- Secondary data use (tracking, profiling, model training)  
- Creation of large-scale identity databases

**GCAC Approach:**  
Only a **government-issued eligibility token** is shared. No date of birth, ID number, biometrics, or other PII is disclosed to the service provider.

---

## ❌ 2. Cross-Service Tracking

Reusable credentials across multiple services enable cross-site correlation, effectively creating a universal identifier.

**GCAC Approach:**  
Tokens are **site-scoped**:

- Each relying party (RP) receives a different cryptographic token  
- Tokens cannot be correlated across services

---

## ❌ 3. VPN and Proxy Circumvention

Location-based enforcement is easily bypassed with VPNs. Some governments are considering restricting VPN usage as a result. ([TechRadar][9])

**GCAC Approach:**  
Eligibility is **identity-based, not location-based**.

- VPN use does not change age eligibility  
- Even if VPN providers were regulated, they could verify eligibility using GCAC tokens without storing identity data

---

## ❌ 4. Platform Fragmentation

Today, each platform must design its own age-check system:

- ID uploads  
- Facial estimation  
- Payment verification  

This favors large companies and leads to inconsistent, error-prone implementations. ([Reddit][10])

**GCAC Approach:**  
A standardized, government-backed protocol reduces compliance burden and improves consistency.

---

## ❌ 5. Weak Protection Despite Intrusion

Even with invasive checks, minors frequently bypass systems using simple workarounds. ([Reddit][11])

**GCAC Approach:**  
Age eligibility is based on **verified government identity assertions**, not AI estimation or heuristics.

---

# How GCAC Addresses These Issues

## 👤 Privacy by Design

GCAC uses cryptographically derived tokens that:

- Are anonymous to service providers  
- Reveal only legal eligibility  
- Prevent identity disclosure and cross-site tracking  

This aligns with data minimization principles in GDPR.

---

## 🔒 Secure, Standardized Attestations

GCAC improves security by:

- Using government authorities as trusted issuers  
- Binding tokens to client identity and scope  
- Requiring proper client authentication and registered callbacks  

---

## 💼 Compliance Without Data Hoarding

Because platforms receive only eligibility assertions:

- They do not need to store identity databases  
- They become less attractive targets for attackers  
- They avoid reliance on invasive third-party vendors

---

## 🛡️ VPN-Resilient Design

Since GCAC verifies *who* is eligible rather than *where* they are:

- VPNs do not enable underage access  
- Regulatory expansion to VPN services would still preserve user privacy through token-based eligibility

---

# Summary Comparison

| Policy Goal                 | Traditional Systems            | GCAC                               |
|-----------------------------|--------------------------------|------------------------------------|
| Protect children online     | Inconsistent, intrusive        | Strong, privacy-preserving         |
| Prevent VPN circumvention   | Weak (location-based)          | Strong (identity-based)            |
| Reduce tracking             | Reusable identifiers common    | Site-scoped tokens                 |
| Consistency across platforms| Fragmented                     | Standardized protocol              |
| Auditability                | Limited                        | High                               |

---

# Limitations and Trade-offs

## ⛔ Requires Trust in Government Issuer
Users and platforms must trust the issuing authority’s security and fairness.

## ⛔ Deployment Complexity
Requires:

- Government infrastructure  
- Service provider adoption  
- Ongoing operational support  

## ⛔ Not Perfect Against Credential Sharing
Users could still share or steal credentials, though GCAC reduces many common abuse vectors.

---

# Conclusion

Current regulatory approaches often:

- Depend on intrusive private-sector data collection  
- Produce fragmented, inconsistent implementations  
- Are vulnerable to circumvention  

**GCAC provides a privacy-preserving, standardized, and regulator-aligned alternative** that enables age and eligibility enforcement **without mass identity sharing or commercial surveillance**.

---

[1]: https://www.gov.uk/government/news/keeping-children-safe-online-changes-to-the-online-safety-act-explained  
[2]: https://www.techradar.com/vpn/vpn-privacy-security/age-verification-requirements-have-landed-in-the-uk-how-the-internet-will-change-and-what-about-your-privacy  
[3]: https://www.esafety.gov.au/about-us/industry-regulation/social-media-age-restrictions/faqs  
[4]: https://www.reuters.com/legal/litigation/australia-social-media-ban-takes-effect-world-first-2025-12-09/  
[5]: https://www.infrastructure.gov.au/sites/default/files/documents/osar-submission-112-digital-rights-watch.pdf  
[6]: https://safepaper.io/safety-guide/age-verification/  
[7]: https://fightchatcontrol.eu/  
[9]: https://www.techradar.com/vpn/vpn-privacy-security/could-vpns-be-banned-uk-government-to-look-very-closely-into-their-usage-amid-mass-usage-since-the-age-verification-row  
[10]: https://www.reddit.com/r/australian/comments/1mcp9cv  
[11]: https://www.reddit.com/r/aussie/comments/1pkmn78/australian_kids_finding_ways_around_countrys_new/  
[12]: https://www.infrastructure.gov.au/sites/default/files/documents/government-response-to-the-roadmap-for-age-verification-august2023.pdf  
