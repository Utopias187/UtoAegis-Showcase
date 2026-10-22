# UtoAegis

**A proprietary .NET 10 Windows endpoint-security engineering project by Francisco Jaimes**

UtoAegis is an incrementally developed endpoint-security platform focused on
defensive design, explicit trust boundaries, safe file handling, authenticated
threat intelligence, and operationally visible failure modes.

This repository is a portfolio showcase. It contains project documentation only;
the implementation is maintained in private repositories.

## What the project demonstrates

- Streaming SHA-256 file identification without executing scanned files
- Local malicious-hash reputation backed by a versioned SQLite schema
- Bounded exact-byte signature detection across streaming buffer boundaries
- Recursive directory scanning with concurrency, traversal, and reparse-point limits
- Correlated JSON Lines telemetry and transactional detection history
- Debounced real-time monitoring with overflow recovery
- Detection-gated quarantine and integrity-verified restoration
- A supervised Windows Service with an observe-only default
- Signed ECDSA P-256 threat-intelligence bundles with anti-rollback state
- A separately privileged HTTPS updater with retention and rollout telemetry
- Atomic live detector replacement with last-known-good fallback and generation leases

## Architecture

```mermaid
flowchart LR
    CLI[CLI workflows] --> Core[Detection and response contracts]
    Service[Windows monitoring service] --> Monitor[Bounded file monitor]
    Monitor --> Core
    Core --> Hash[SHA-256 scanner]
    Core --> Signature[Byte-signature scanner]
    Hash --> Reputation[(SQLite reputation store)]
    Core --> History[(Detection history)]
    Core --> Vault[Restricted quarantine vault]

    Publisher[Offline bundle publisher] --> Signed[Signed threat bundle]
    Updater[Privileged updater] --> Signed
    Signed --> Content[(Managed content store)]
    Content --> Reload[Read-only live reload]
    Reload --> Core
```

The monitoring service remains read-only toward managed threat content. The
updater is the only component granted content-store write access, while private
signing keys stay off endpoints.

## Milestone progression

| Milestone | Capability |
| --- | --- |
| 1 | File hash scanner |
| 2 | Malware hash reputation database |
| 3 | Streaming signature scanner |
| 4 | Bounded directory scanner |
| 5 | Detection history and structured logging |
| 6 | Real-time file monitoring |
| 7 | Safe quarantine and recovery |
| 8 | Supervised Windows monitoring service |
| 9 | Authenticated threat-intelligence updates |
| 10 | Production threat-intelligence updater |
| 11 | Live threat-intelligence reloading |
| 11.1 | Read-only reload polling correction |

## Security-engineering highlights

- Files are scanned without execution.
- Results distinguish detection, no match, unknown reputation, and operational failure.
- Directory traversal rejects unsafe reparse-point behavior and enforces explicit bounds.
- Quarantine requires detector evidence and revalidates file identity before removal.
- Threat bundles bind exact manifest bytes, hashes, lengths, versions, and validity windows.
- Monotonic sequence state prevents replay of older signed content.
- Candidate detector generations are completely validated before atomic publication.
- Invalid updates preserve the last-known-good in-memory generation.
- Monitoring and updating run under separate least-privilege service identities.

## Verification snapshot

The Milestone 11.1 baseline was verified with:

- .NET 10 Release build with zero warnings and zero errors
- 249 passing automated tests across ten projects
- Locked dependency restoration
- Direct and transitive NuGet vulnerability auditing
- Formatter verification
- PowerShell parser validation for all deployment scripts
- Successful GitHub Actions runs for the main and standalone milestone snapshots

This is an engineering portfolio project, not a claim of independent antivirus
certification or guaranteed detection of every malicious file.

## Technology

C#, .NET 10, SQLite, MSTest, Windows Services, PowerShell, GitHub Actions,
ECDSA P-256, SHA-256, JSON Lines, and Windows filesystem security controls.

## Ownership

Copyright © 2026 Francisco Jaimes. All rights reserved.

UtoAegis is proprietary software. The source implementation is not licensed for
copying, modification, redistribution, or third-party commercial use. This
showcase does not distribute the source code or grant implementation rights.

## Contact

- [GitHub profile](https://github.com/Utopias187)
- [LinkedIn](https://www.linkedin.com/in/frjaimes/)
