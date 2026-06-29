# Finding 03: No Multi-Factor Authentication (MFA)

## Severity: High (CVSS 7.2)

## Vulnerability Description

The application does not offer or enforce multi-factor authentication for user accounts, including accounts with administrative or privileged access. Authentication relies solely on a single factor — username and password.

## Technical Details

- **Affected Component:** Authentication system (all user roles, including admin accounts)
- **Attack Vector:** Single-factor authentication; any compromised credential set (via phishing, credential stuffing, or data breach reuse) grants full account access
- **Proof of Concept (illustrative):** Using a credential pair obtained from a known breach dump (credential stuffing) successfully logs in with no secondary verification step
- **CWE Reference:** CWE-308 (Use of Single-factor Authentication)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.5 — Secure Authentication
  Secure authentication mechanisms, including multi-factor authentication for sensitive or privileged access, are a direct requirement of this control.

## Business Risk Statement

Without MFA, a single leaked or guessed password is sufficient for full account compromise. This is particularly severe for administrative accounts, where compromise could lead to data manipulation, configuration changes, or full system takeover. Note that this finding is not independently exploitable — it amplifies the impact of other weaknesses, such as weak lockout policies and lack of rate limiting (Findings 06 and 07), which together create a significantly higher combined risk than any one finding alone.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | High (7.2) |
| Business Impact | High — single point of failure for account compromise |
| Exploitability | Medium — requires already-compromised credentials |
| **Overall Priority** | **High — remediate within 1-2 weeks** |

## Recommended Remediation

- Implement MFA (TOTP-based authenticator app or hardware token) for all user accounts
- Enforce MFA as mandatory for administrative and privileged accounts at minimum, even if optional elsewhere
- Consider risk-based/adaptive authentication (e.g., requiring MFA on new device or location)
- Communicate the rollout to users with clear setup guidance to reduce support burden
