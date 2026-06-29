# Finding 09: Self-Signed SSL Certificate

## Severity: Low (CVSS 3.7)

## Vulnerability Description

The application uses a self-signed SSL certificate rather than one issued by a trusted Certificate Authority (CA). This causes browser trust warnings and undermines the certificate trust chain that TLS relies on to verify server identity.

## Technical Details

- **Affected Component:** Web server TLS certificate configuration
- **Attack Vector:** Lack of trusted CA-issued certificate; requires an attacker to be positioned on the network path and the user to bypass or ignore browser trust warnings
- **Proof of Concept (illustrative):** Browser displays a certificate trust warning when accessing the application over HTTPS, indicating the certificate is not issued by a recognized CA
- **CWE Reference:** CWE-295 (Improper Certificate Validation)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.24 — Use of Cryptography
  Certificate management is part of the broader cryptographic control requirements; using a self-signed certificate in a production environment falls short of expected certificate management practices.

## Business Risk Statement

This finding does not directly enable exploitation on its own — it primarily indicates weak certificate management practices and trains users to ignore security warnings, which has a secondary risk of normalizing dismissal of legitimate browser warnings (potentially aiding future phishing or man-in-the-middle attempts). It also undermines user trust and could affect perceived legitimacy of the application.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | Low (3.7) |
| Business Impact | Low — primarily a trust/hygiene issue rather than directly exploitable |
| Exploitability | Low — requires network position and user to dismiss browser warnings |
| **Overall Priority** | **Low — remediate within 4-6 weeks** |

## Recommended Remediation

- Replace the self-signed certificate with one issued by a trusted Certificate Authority (e.g., via Let's Encrypt for a free, automated option)
- Implement automated certificate renewal to prevent future expiration-related issues
- Ensure certificate covers all relevant subdomains if applicable
