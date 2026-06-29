# Finding 04: Broken Access Control (Admin Functions)

## Severity: High (CVSS 8.1)

## Vulnerability Description

The application fails to enforce role-based access restrictions on the server side. A standard authenticated user can access administrative API endpoints directly (e.g., by calling the endpoint URL/method even though no UI link is exposed to them), because authorization checks are missing or rely solely on hiding UI elements rather than validating permissions server-side.

## Technical Details

- **Affected Component:** Administrative API endpoints (e.g., `/api/v1/admin/users`, `/api/v1/admin/settings`)
- **Attack Vector:** Missing server-side authorization checks; client-side-only restriction (UI hiding) is not a security control
- **Proof of Concept (illustrative):** A standard user account, when directly calling `/api/v1/admin/users` with their existing session token, receives a valid response instead of a 403 Forbidden
- **CWE Reference:** CWE-862 (Missing Authorization)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.5.15 — Access Control
  Access control policy and enforcement should restrict access based on role; this was not enforced at the application layer.
- **Secondary Control:** A.8.2 — Privileged Access Rights
  Privileged functions were accessible without verification of privileged status, violating the principle of restricting privileged access to authorized roles only.

## Business Risk Statement

This allows any authenticated standard user to escalate privileges and access or modify administrative functions — including potentially viewing all user data, changing system configurations, or disabling security controls. Because exploitation only requires a standard (non-privileged) account, the barrier to exploitation is low while the potential impact is severe, making this a high-priority finding despite not being rated Critical.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | High (8.1) |
| Business Impact | High — data manipulation, privilege escalation, configuration risk |
| Exploitability | Medium — requires only a standard authenticated account |
| **Overall Priority** | **High — remediate within 1-2 weeks** |

## Recommended Remediation

- Implement server-side authorization checks on every administrative endpoint, independent of UI visibility
- Apply the principle of least privilege: explicitly verify role/permission before processing any privileged request
- Conduct an access control review across all API endpoints, not just the ones identified in this assessment
- Add automated authorization testing to the CI/CD pipeline to catch regressions
