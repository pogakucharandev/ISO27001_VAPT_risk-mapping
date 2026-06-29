# Finding 07: No Rate Limiting

## Severity: Medium (CVSS 5.8)

## Vulnerability Description

The application does not enforce rate limiting on sensitive endpoints, including login, password reset, and API requests. This allows an attacker to send a high volume of automated requests in a short period, enabling brute-force attacks, credential stuffing, and resource exhaustion.

## Technical Details

- **Affected Component:** Login endpoint, password reset endpoint, and general API layer
- **Attack Vector:** Absence of request throttling based on IP, account, or session
- **Proof of Concept (illustrative):** Sending several thousand login requests within a few minutes from a single source without triggering any throttling, CAPTCHA, or blocking response
- **CWE Reference:** CWE-799 (Improper Control of Interaction Frequency)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.5 — Secure Authentication
  Authentication endpoints should be protected against high-volume automated abuse.
- **Secondary Control:** A.8.16 — Monitoring Activities
  Without throttling, abusive request patterns are also less likely to be flagged or detected through normal monitoring, since there's no threshold being breached to trigger alerts.

## Business Risk Statement

This enables brute-force and credential stuffing attacks at scale, particularly dangerous in combination with Findings 03 (No MFA) and 06 (Weak Account Lockout). Beyond credential attacks, the lack of rate limiting on general API endpoints also creates a resource exhaustion / denial-of-service risk if exploited at volume.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | Medium (5.8) |
| Business Impact | Medium — enables large-scale credential attacks and potential service degradation |
| Exploitability | Medium — requires automation tooling, low skill barrier |
| **Overall Priority** | **Medium — remediate within 2-4 weeks** |

## Recommended Remediation

- Implement rate limiting on authentication-related endpoints (login, password reset, OTP verification) based on IP and/or account
- Apply progressive delays or CAPTCHA challenges after a threshold of requests is reached
- Extend rate limiting to general API endpoints to mitigate resource exhaustion risk
- Monitor and alert on requests exceeding defined thresholds
