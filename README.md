# VAPT to ISO 27001 Risk Mapping

A practical demonstration of translating technical vulnerability findings (from web application VAPT) into governance, risk, and compliance (GRC) language — mapped to ISO/IEC 27001:2022 Annex A controls.

## Why this project exists

Most security engineers stop at finding and reporting vulnerabilities. Most GRC analysts work from frameworks without deep technical grounding. This project bridges that gap: it shows how a technical finding translates into a business risk statement, a specific Annex A control failure, and a prioritized remediation action — the way a security risk consultant or GRC analyst actually needs to think.

**Disclaimer:** All findings in this repository are synthetic and created for demonstration purposes only. They do not represent any real client engagement, system, or confidential data. See [disclaimer.md](./disclaimer.md).

## What's inside

| File/Folder | Purpose |
|---|---|
| [`risk-register.csv`](./risk-register.csv) | Consolidated risk register — all findings, controls, and priorities in one view |
| [`methodology.md`](./methodology.md) | How findings were rated, mapped, and prioritized |
| [`findings/`](./findings) | Individual detailed write-ups for each vulnerability |

## Summary: Risk Register

| # | Finding | Severity | Annex A Control(s) |
|---|---|---|---|
| 1 | Malicious File Upload | Critical | A.8.28, A.8.29 |
| 2 | Stored XSS | High | A.8.28, A.8.26 |
| 3 | No MFA | High | A.8.5 |
| 4 | Broken Access Control (Admin Functions) | High | A.5.15, A.8.2 |
| 5 | Weak SSL Configuration | Medium | A.8.24 |
| 6 | Weak Account Lockout | Medium | A.8.5 |
| 7 | No Rate Limiting | Medium | A.8.5, A.8.16 |
| 8 | Session Timeout Misconfiguration | Medium | A.8.5 |
| 9 | Self-Signed Certificate | Low | A.8.24 |
| 10 | Missing Security Headers | Low | A.8.26 |

**Severity distribution:** 1 Critical, 3 High, 4 Medium, 2 Low — intentionally weighted toward Medium/Low, reflecting realistic web application VAPT engagements rather than an exaggerated "everything is Critical" report.

## Notable control clustering

Two clusters of overlapping controls appear deliberately, not by error:

- **A.8.24 (Cryptography):** Weak SSL Config and Self-Signed Certificate both relate to cryptographic control failure, but represent distinct issues — protocol-level weakness vs. certificate trust configuration.
- **A.8.5 (Secure Authentication):** Weak Account Lockout, No Rate Limiting, and Session Timeout all sit under authentication hardening, but each represents a separate, independently exploitable weakness.

Full reasoning in [methodology.md](./methodology.md).

## About me

I'm a security engineer working on web application VAPT. This project reflects how I approach connecting technical findings to governance frameworks — built while preparing for a transition into GRC/security risk consulting roles.

[LinkedIn] · [Email]
