# 🔐 Digital Certificates & SHA-256 Flag Validation

Labs and coursework from **Intro to Applied Cryptography** at OSU-IT, covering SSL/TLS/SSH/IPsec comparisons, digital certificate creation, and cryptographic hash validation.

**Instructor:** Brett Webber | **Course:** Intro to Applied Cryptography

---

## Digital Certificate Creation

### What is a Digital Certificate?

A digital certificate (also called a public-key certificate) binds a public key to an identity. It is signed by a trusted Certificate Authority (CA) and used to establish secure, authenticated communications — most commonly in HTTPS via TLS/SSL.

### SSL/TLS Handshake — How Certificates Are Validated

When an SSL client connects to a server, it validates the server's certificate by:
1. Verifying all certificates in the chain of trust back to a trusted root CA
2. Checking relevant Certificate Revocation Lists (CRLs) for revoked certificates
3. Confirming the certificate's validity period and domain match

> **Self-signed certificates** — such as one a university department might create for its own web server — bypass the CA chain. The browser has no way to verify that the certificate was issued by a trusted authority, leaving the connection vulnerable to man-in-the-middle attacks where an attacker could intercept traffic by presenting their own certificate.

### Real-World Certificate Risks

| Scenario | Risk |
|---|---|
| Self-signed certificate on a web server | No third-party trust verification; susceptible to MITM attacks |
| Third-party payment provider on a government site | Data breach risk if provider is compromised; potential for fake payment pages |
| Weak CA trust model in browsers | Overreliance on centralized CAs; a compromised CA can issue fraudulent certs |

### SSL vs. SSH vs. IPsec — Comparison

| | SSL/TLS | SSH | IPsec |
|---|---|---|---|
| **OSI Layer** | Transport | Application | Network (Internet) |
| **Security Services** | Authentication, integrity, privacy, non-repudiation | Integrity, anti-eavesdropping, anti-spoofing | Confidentiality, authentication |
| **Algorithms** | AES, 3DES, RC4 | 3DES | AES, 3DES, DES, CAST, Blowfish |
| **Real-World Use** | Banking, social media, healthcare (HTTPS) | Remote server administration, VPN tunneling | Medical records, VPN tunneling (ESP) |
| **Key Management** | 40–128 bit session keys | Auto-generated key pairs | 56–256 bit encryption keys |

---

## TLS Version Recommendations

| Version | Status |
|---|---|
| SSL 2.0 / 3.0 | ❌ Deprecated — do not use |
| TLS 1.0 / 1.1 | ❌ Deprecated — vulnerable |
| TLS 1.2 | ✅ Widely adopted, considered secure |
| TLS 1.3 | ✅ Recommended — improved security & performance |

Per NIST guidance, government-facing servers must support TLS 1.2 and TLS 1.3, and **shall not** use TLS 1.0, SSL 3.0, or SSL 2.0. TLS 1.3 support was mandated by January 1, 2024.

---

## SHA-256 Flag Validation

### What is SHA-256?

SHA-256 (Secure Hash Algorithm 256-bit) is a cryptographic hash function that produces a fixed 256-bit (64 hex character) digest from any input. It is widely used for:
- File integrity verification
- Password hashing
- Digital signatures
- CTF (Capture the Flag) challenge validation

### Why SHA-256?

SHA-256 is part of the SHA-2 family and is considered cryptographically secure for most applications. Unlike MD5 or SHA-1, no practical collision attacks have been demonstrated against SHA-256.

### Validating a File Hash

To verify a downloaded file hasn't been tampered with, compute its SHA-256 hash and compare it to the published value:

**Linux/macOS:**
```bash
sha256sum filename.exe
# or
shasum -a 256 filename.exe
```

**Windows (PowerShell):**
```powershell
Get-FileHash filename.exe -Algorithm SHA256
```

### Lab Example — Malware Hash Validation

In the Wireshark HTTP traffic lab, a Windows executable was identified that was downloaded over HTTP. Its SHA-256 hash was extracted and submitted to VirusTotal for threat intelligence:

```
SHA256: 8d5d36c8ffb0a9c81b145aa40c1ff3475702fb0b5f9e08e0577bdc405087e635
```

Submitting this hash to [VirusTotal](https://www.virustotal.com) revealed the detection rate across multiple antivirus engines, confirming the file as malicious.

---

## GSM/UMTS Mobile Security Notes

| Topic | Key Point |
|---|---|
| SIM card role | Stores the IMSI (subscriber identity) and a unique 128-bit cryptographic key issued by the carrier |
| End-to-end encryption | GSM/UMTS are **not** designed for end-to-end security — calls can be intercepted once routed to the PSTN |
| Public-key cryptography | Not used because all key material is pre-loaded onto SIM cards before distribution |

---

## EMV (Chip Card) Cryptography

EMV (Europay, Mastercard, Visa) is the global standard for chip-based payment cards. It is largely symmetric-key based, but uses public-key cryptography in specific areas:

- **Static Data Authentication:** The card stores a public-key certificate signed by the Payment Card Organization (PCO). Terminals validate this certificate using the PCO's pre-installed verification key.
- **Non-repudiation mechanisms:**
  - *Identify* — computes a value from the card's symmetric key + a transaction counter
  - *Response* — bank issues a challenge; card computes a response using its symmetric key
  - *Sign* — a stronger version using CBC-MAC over transaction data, functioning as a digital signature
- **Why not AES?** EMV uses 2TDES and RSA because they are well-established, extensively tested algorithms with broad hardware support. Migrating to AES requires significant infrastructure changes.
