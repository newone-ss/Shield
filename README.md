# Valence_IPSec (NTRO Problem Statement 26160 — SIH 2026)
### AI-Powered IPsec VPN Protocol Analyzer & Security Assessment Framework

TunnelTwin is a next-generation protocol assessment and automated hardening framework designed for enterprise, defense, and mission-critical IPsec VPN deployments.

---

## Key Capabilities

1. **Four-Tier Provenance Tracking**:
   Every reported finding, parameter, and rule evaluation explicitly tags its origin:
   - `observed`: Discovered through safe active proposal probing.
   - `parsed`: Extracted from static configuration files (Cisco IOS, strongSwan, FortiOS).
   - `inferred`: Predicted by heuristic machine learning with confidence scores $\in [0.0, 1.0]$.
   - `unknown`: Missing or unreachable parameters. Rules requiring unknown facts report `"cannot assess"`, never a false pass.

2. **Safety-First, Consent-Gated Active Probing**:
   Non-intrusive cryptographic proposal sweeps restricted strictly to pre-authorized hosts with verified consent flags. Zero aggressive mode harvesting, zero credential brute-forcing.

3. **Multi-Standard Compliance Engine**:
   Evaluates against NIST SP 800-77 Rev 1, ANSSI, RFC 9395, and CNSA 2.0 quantum-resistant guidelines.

4. **Automated Remediation & Unified Diffs**:
   Translates compliance violations into vendor-specific, hardened configuration snippets with colorized unified diffs and parser round-trip verification.

5. **Cryptographic Merkle Audit Trail**:
   Every scan, assessment, and remediation creates an immutable SHA-256 Merkle tree log, providing verifiable proof of assessment integrity.

---

## Directory Architecture

- `tunneltwin/core/`: Normalized IPsec schemas, provenance tracking models, and shared utilities.
- `tunneltwin/ike/`: IKEv1/IKEv2 binary packet definitions and cryptographic suite tables.
- `tunneltwin/probe/`: Consent-gated active IKE probing engine.
- `tunneltwin/rules/`: Compliance policy rules (NIST SP 800-77r1, ANSSI, RFC 9395).
- `tunneltwin/fix/`: Automated remediation and unified diff generator.
- `tunneltwin/capture/`: PCAP / live network traffic parser.
- `tunneltwin/ml/`: Parameter inference engine with confidence scoring.
- `tunneltwin/seal/`: Cryptographic Merkle audit trail and receipt generator.
- `tunneltwin/api/`: FastAPI REST backend.
- `tunneltwin/cli/`: Interactive command-line interface.
- `tunneltwin/ui/`: Modern cyber-aesthetic web dashboard.
- `lab/`: Linux network namespace testbed automation and swanctl profiles.
- `tests/`: Automated unit, integration, and round-trip test suites.
- `docs/`: In-depth architectural specifications and guides.
