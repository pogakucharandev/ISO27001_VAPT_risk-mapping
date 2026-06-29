# Finding 05: Weak SSL/TLS Configuration

## Severity: Medium (CVSS 5.9)

## Vulnerability Description

The web server supports outdated and weak TLS protocol versions (e.g., TLS 1.0/1.1) and/or weak cipher suites alongside modern, secure configurations. This allows a downgrade attack where a man-in-the-middle attacker can force a connection to use the weaker, more exploitable protocol or cipher.

## Technical Details

- **Affected Component:** Web server TLS configuration (all HTTPS endpoints)
- **Attack Vector:** Protocol/cipher downgrade requiring an attacker to be positioned on the network path (e.g., public Wi-Fi, compromised router)
- **Proof of Concept (illustrative):** SSL/TLS scan (e.g., via `testssl.sh` or `nmap --script ssl-enum-ciphers`) reveals support for TLS 1.0 and weak cipher suites such as those using RC4 or CBC-mode ciphers vulnerable to known attacks
- **CWE Reference:** CWE-327 (Use of a Broken or Risky Cryptographic Algorithm)

## ISO 27001 Annex A Control Mapping

- **Primary Control:** A.8.24 — Use of Cryptography
  Cryptographic protocols and algorithms in use must meet defined security standards; outdated protocols and weak ciphers fail this requirement.

## Business Risk Statement

Under specific network conditions (such as an attacker positioned on the same network, e.g., public Wi-Fi), this weakness could allow interception or downgrade of encrypted traffic, exposing data in transit including session tokens or credentials. The risk is conditional on network position rather than directly exploitable from the internet at large, which is why this is rated Medium rather than High.

## Risk Priority Rating

| Factor | Rating |
|---|---|
| Technical Severity (CVSS) | Medium (5.9) |
| Business Impact | Medium — data-in-transit exposure under specific conditions |
| Exploitability | Low — requires network position (man-in-the-middle) |
| **Overall Priority** | **Medium — remediate within 2-4 weeks** |

## Recommended Remediation

- Disable TLS 1.0 and 1.1; support TLS 1.2 and 1.3 only
- Remove weak cipher suites (e.g., RC4, DES, CBC-mode ciphers known to be vulnerable) from the server configuration
- Use a tool such as Mozilla's SSL Configuration Generator to apply a modern, vetted configuration baseline
- Periodically re-scan TLS configuration as part of routine security maintenance
