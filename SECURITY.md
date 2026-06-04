# Security & Disclosure Status

This repository **is itself a coordinated vulnerability disclosure** for the
DigiDoc4 Client (macOS). It contains:

- `ADVISORY.txt` — the full security advisory.
- `README.md` — reproduction instructions.
- A single proof-of-concept container file whose **filename is the payload**
  (a deliberately benign rickroll). See `ADVISORY.txt` §4.

## Disclosure status

> **Private disclosure — do not republish before the fix is released.**

| | |
|---|---|
| Vulnerability | AppleScript injection via container filename in macOS `mailto:` handler |
| CWE | CWE-94 (Code Injection) |
| Affected | DigiDoc4 Client, all current macOS versions |
| Reported | 14.05.2026 |
| Primary channel | CERT-EE — cert@cert.ee (forwarded to RIA) |
| Public disclosure window | Day 90 reference (§9), extendable on reasonable request |

The `open-eid/DigiDoc4-Client` repository does not currently expose GitHub
private vulnerability reporting (`/security/advisories/new` returns 404), so
the report was submitted to CERT-EE instead.

## Reporting issues with *this* repository

For anything about the advisory itself, contact the reporter:

- Mark Zelinski — mazeli@taltech.ee

## Safe-handling note

The PoC file is byte-identical to a valid signed container; only its
**filename** carries the AppleScript payload, and that payload only does
anything when opened in a vulnerable DigiDoc4 build and the "Send by email"
button is clicked. It is otherwise inert. Even so, treat the filename as
live exploit input: do not pass it to shells, mail clients, or AppleScript
tooling unintentionally.
