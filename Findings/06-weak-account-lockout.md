# Finding 06: Weak Account Lockout Policy

## Severity: Medium (CVSS 5.3)

## Vulnerability Description

The application does not lock or sufficiently delay accounts after repeated failed login attempts. An attacker can make a large number of password guesses against a target account without triggering a lockout, significantly increasing the feasibility of brute-force or credential-guessing attacks.

## Technical Details

- **Affected Component:** Login authentication endpoint
- **Attack Vector:** Absence of, or excessively high threshold for, account lockout after failed login attempts
- **Proof of Concept (illustrative):** Submitting 100+ consecutive incorrect password attempts against a single account without triggering any lockout, delay, or alert
- **CWE Reference:** CWE-307 (Improper Restriction of Excessive Authentication Attempts)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.5 — Secure Authentication
  Secure authentication mechanisms should include protections against brute-force attempts, such as lockout or progressive delay policies.

## Business Risk Statement

This weakness increases the feasibility of brute-force attacks against user accounts, particularly when combined with commonly reused or weak passwords. This finding is part of an authentication hardening cluster alongside Findings 07 (No Rate Limiting) and 03 (No MFA) — individually each is a Medium or High risk, but combined, they substantially lower the effort required for an attacker to compromise an account.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | Medium (5.3) |
| Business Impact | Medium — increases brute-force feasibility |
| Exploitability | Medium — requires sustained automated attempts |
| **Overall Priority** | **Medium — remediate within 2-4 weeks** |

## Recommended Remediation

- Implement account lockout after a reasonable number of failed attempts (e.g., 5-10), with a temporary lockout period or progressive delay
- Avoid permanent lockouts that could enable denial-of-service against legitimate users; use time-based lockouts instead
- Log and alert on repeated failed login attempts for security monitoring purposes
- Combine with rate limiting (Finding 07) for stronger protection
