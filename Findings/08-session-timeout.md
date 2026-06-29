# Finding 08: Session Timeout Misconfiguration

## Severity: Medium (CVSS 4.8)

## Vulnerability Description

The application does not enforce a reasonable session timeout. Sessions remain valid for an extended period (or indefinitely) regardless of user inactivity, increasing the window during which a stolen or unattended session could be exploited.

## Technical Details

- **Affected Component:** Session management (all authenticated sessions)
- **Attack Vector:** Excessively long or absent idle session timeout, increasing exposure window for session hijacking or misuse on shared/unattended devices
- **Proof of Concept (illustrative):** A session remains active and valid more than 24 hours after the last user activity, with no re-authentication prompt
- **CWE Reference:** CWE-613 (Insufficient Session Expiration)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.5 — Secure Authentication
  Session management, including timeout enforcement, falls under secure authentication practices in the 27001:2022 Annex A structure.

## Business Risk Statement

While not independently exploitable, this finding extends the window of opportunity for session hijacking attacks — particularly relevant on shared computers, public terminals, or in scenarios where a session token is exposed through another vulnerability (such as Finding 02, Stored XSS). The risk is one of increased exposure duration rather than a direct attack vector on its own.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | Medium (4.8) |
| Business Impact | Medium — extends exposure window for session-based attacks |
| Exploitability | Low — requires a separate vector (shared device or session theft) to actually exploit |
| **Overall Priority** | **Medium — remediate within 4 weeks** |

## Recommended Remediation

- Implement a reasonable idle session timeout (e.g., 15-30 minutes for standard users, shorter for privileged accounts)
- Implement an absolute session expiration regardless of activity (e.g., re-authentication required after 8-12 hours)
- Invalidate session tokens server-side on logout, not just client-side
- Consider step-up authentication for sensitive actions even within an active session
