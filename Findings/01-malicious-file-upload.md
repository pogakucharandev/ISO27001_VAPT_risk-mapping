# Finding 01: Malicious File Upload

## Severity: Critical (CVSS 9.4)

## Vulnerability Description

The file upload feature on the user profile page (`/api/v1/profile/upload`) does not validate file type, content, or extension before storing uploaded files in a web-accessible directory. An attacker can upload an executable script (e.g., a web shell) disguised with an allowed extension or bypassing client-side validation entirely.

## Technical Details

- **Affected Component:** Profile picture upload endpoint
- **Attack Vector:** Lack of server-side file type validation and content inspection
- **Proof of Concept (illustrative):** Uploading a file named `shell.php.jpg` or manipulating the Content-Type header to bypass extension checks, resulting in a web shell accessible at a predictable path
- **CWE Reference:** CWE-434 (Unrestricted Upload of File with Dangerous Type)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.28 — Secure Coding
  Input validation for file uploads is a secure coding requirement; this control was not implemented.
- **Secondary Control:** A.8.29 — Security Testing in Development and Acceptance
  A vulnerability of this severity should have been identified during pre-release security testing (SAST/DAST or manual review).

## Business Risk Statement

This vulnerability allows an attacker to execute arbitrary code on the server, potentially leading to full system compromise. From there, an attacker could access the underlying database, pivot to other internal systems, deface the application, or use the server as a foothold for further attacks. Given the potential for complete data breach and service disruption, this represents an immediate, severe business risk — not a theoretical one.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | Critical (9.4) |
| Business Impact | Critical — full system compromise possible |
| Exploitability | High — no authentication required, low attacker skill needed |
| **Overall Priority** | **Critical — remediate within 24–48 hours** |

## Recommended Remediation

- Implement server-side file type validation using content inspection (magic bytes), not just file extension
- Store uploaded files outside the web root, or serve them through a handler that prevents script execution
- Apply strict file size limits and allowed file type whitelisting
- Add this vulnerability class to automated security testing in the CI/CD pipeline
- Conduct a retest after remediation to confirm the fix
