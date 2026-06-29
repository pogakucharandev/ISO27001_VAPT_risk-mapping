# Methodology

This document explains how findings in this project were rated, mapped to ISO/IEC 27001:2022 Annex A controls, and prioritized. The goal is to make the thinking behind each decision visible, not just the conclusions.

## 1. Severity Rating

Severity was assigned using CVSS v3.1 as a reference point, then adjusted based on realistic exploitability and business context — not taken as a fixed score in isolation.

A few specific judgment calls worth noting:

- **No MFA** is rated High, not Critical. On its own, missing MFA is a control gap, not an active compromise — it requires another weakness (a leaked password, no rate limiting, weak lockout) to actually be exploited. Rating it Critical would overstate standalone risk.
- **Malicious File Upload** is rated Critical specifically because it can lead to remote code execution if exploited (e.g., uploading a web shell). Without that potential, this finding would be High at most.
- **Self-Signed Certificate** is rated Low, not Medium. It's a configuration hygiene issue and certificate trust problem, not an active exploit path by itself.

## 2. Severity Distribution

Real web application VAPT engagements rarely return mostly Critical findings. This project intentionally reflects a realistic distribution — weighted toward Medium and Low — rather than an exaggerated spread designed to look more impressive than it would be in practice.

Final distribution: 1 Critical, 3 High, 4 Medium, 2 Low.

## 3. Annex A Control Mapping

Each finding was mapped to the Annex A control it most directly violates. Where a finding plausibly touches more than one control, a secondary control is listed — but only where it adds meaningful context, not for the sake of completeness.

## 4. Handling Control Overlap

Two clusters of findings map to the same primary control. This was identified deliberately and called out rather than hidden or avoided:

**Cryptography cluster (A.8.24):**
- Weak SSL Configuration — protocol/cipher-level weakness (e.g., outdated TLS versions)
- Self-Signed Certificate — trust chain/certificate management weakness

These are distinct failure modes within the same control area. Treating them as duplicates would be inaccurate; treating them as unrelated would miss the underlying pattern.

**Authentication hardening cluster (A.8.5):**
- Weak Account Lockout
- No Rate Limiting
- Session Timeout Misconfiguration

All three relate to authentication control strength, but each is independently exploitable and would need a separate remediation action. Grouping them conceptually (without merging them as findings) reflects how a real risk register should communicate this to stakeholders — as a thematic weakness in authentication controls, not three unrelated issues.

## 5. Business Risk Framing

Each finding's business risk statement was written to avoid generic language ("this is bad practice") and instead state a specific consequence: data exposure, regulatory implications, account compromise pathway, or operational impact. This reflects how a risk statement should read in an actual report — specific enough that a non-technical stakeholder understands why the finding matters.

## 6. Prioritization Logic

Priority was determined by combining technical severity with:
- Exploitability (does it require authentication, user interaction, or chained vulnerabilities?)
- Business impact (data sensitivity, regulatory exposure, reputational risk)
- Detectability (would this be caught by monitoring, or does it fail silently?)

This means technical severity and business priority don't always move in lockstep — a Medium CVSS finding combined with high business impact can outrank a technically higher CVSS finding with low real-world exploitability.
