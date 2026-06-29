# Finding 10: Missing Security Headers

## Severity: Low (CVSS 3.1)

## Vulnerability Description

The application does not implement recommended HTTP security headers, including Content-Security-Policy (CSP), X-Frame-Options, X-Content-Type-Options, and Strict-Transport-Security (HSTS). These headers provide browser-level defense-in-depth against several classes of attacks.

## Technical Details

- **Affected Component:** HTTP response headers (application-wide)
- **Attack Vector:** Absence of headers that mitigate clickjacking, MIME-sniffing attacks, and content injection; this is an enabling weakness for secondary attacks rather than directly exploitable on its own
- **Proof of Concept (illustrative):** Inspecting HTTP response headers (e.g., via browser developer tools or `curl -I`) shows no `Content-Security-Policy`, `X-Frame-Options`, or `Strict-Transport-Security` headers present
- **CWE Reference:** CWE-1021 (Improper Restriction of Rendered UI Layers, related to clickjacking)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.26 — Application Security Requirements
  Security headers should be defined as part of application security requirements during the design phase, establishing baseline browser-level protections.

## Business Risk Statement

While not directly exploitable, the absence of these headers increases susceptibility to clickjacking (embedding the application in a malicious iframe to trick users into unintended actions) and reduces defense-in-depth against content injection attacks such as XSS (see Finding 02). This is a best-practice gap that compounds the risk of other findings rather than standing alone as a serious threat.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | Low (3.1) |
| Business Impact | Low — primarily reduces defense-in-depth rather than enabling direct exploitation |
| Exploitability | Low — enables secondary attacks only, not directly exploitable alone |
| **Overall Priority** | **Low — remediate within 4-6 weeks** |

## Recommended Remediation

- Implement `Content-Security-Policy` to restrict allowed sources for scripts, styles, and other resources
- Add `X-Frame-Options: DENY` (or `SAMEORIGIN` if framing is required) to prevent clickjacking
- Add `X-Content-Type-Options: nosniff` to prevent MIME-type sniffing attacks
- Add `Strict-Transport-Security` (HSTS) to enforce HTTPS connections
- Validate header configuration using a tool such as securityheaders.com after implementation
