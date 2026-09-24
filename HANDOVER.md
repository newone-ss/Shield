# TunnelTwin — Session Handover & Continuity Record

> **Living Context Record**: This document is read at the start of every AI session to maintain zero-amnesia continuity across development cycles. It reflects the live operational state of the TunnelTwin project (SIH 2026, Problem Statement 26160 — NTRO: AI-Powered IPsec VPN Protocol Analyzer and Security Assessment Framework).

---

## 1. Current Operational State

- **Active Phase**: Phase 0 Complete — Ready for Phase 1 (Passive Config Parser & Normalizer)
- **Target Deadline**: 29 September 2026
- **Current Objective**: Complete Phase 0 exit verification (PASSED: 4/4 profiles established across network namespaces) and transition to Phase 1.
- **Environment**: Host Windows 11 with WSL2 Ubuntu (`Ubuntu-26.04`), Linux Kernel 6.6.87.2-microsoft-standard-WSL2, full root privileges for netns and IPsec kernel operations.

---

## 2. What Is Done

- **Core Documentation System**:
  - `HANDOVER.md`: Living state and operational context.
  - `DECISIONS.md`: Architectural decision records (ADR-0001 through ADR-0007).
  - `FLOW.md`: Complete data and control flow mapping.
  - `FEATURE.md`: Feature scoping and lifecycle tracking (FEAT-001, FEAT-002, FEAT-003 complete).
  - `BUG.md`: Bug discovery, diagnosis, and fix tracing (BUG-001, BUG-002, BUG-003 resolved).
- **Repository Scaffolding**:
  - `tunneltwin/{core,ike,probe,rules,fix,capture,ml,seal,api,cli,ui}`
  - `lab/`, `tests/`, `docs/`
  - `pyproject.toml` and `README.md`
- **Data Models & Provenance System**:
  - `tunneltwin/core/models.py`: Four-tier provenance tags (`OBSERVED`, `PARSED`, `INFERRED`, `UNKNOWN`), `ProvenancedFact[T]`, confidence scores for inferred facts, and normalized IPsec schema.
- **Lab Substrate & Automation**:
  - `lab/setup_namespaces.sh`: Linux network namespaces (`ns-left`, `ns-right`) linked with veth pair (`10.0.1.1/30` <-> `10.0.1.2/30`).
  - `lab/start_charon.sh`: Spawns isolated strongSwan charons per namespace using `unshare -m` private mount namespaces to prevent `/var/run/charon.pid` collisions.
  - `lab/stop_charon.sh` and `lab/teardown_namespaces.sh`.
  - Four swanctl connection profiles in `lab/configs/`:
    - `weak`: IKEv1, 3DES-SHA1, MODP_1024
    - `mixed`: Prefers AES256/ECP384, accepts MODP_1024
    - `strong`: IKEv2, AES256-GCM, SHA384, ECP384 only
    - `legacy-cbc`: IKEv2, AES128-CBC+SHA1, MODP_2048
  - `lab/run_matrix.sh` and `lab/run_matrix.py`: Automated execution and verification runner.
- **Phase 0 Exit Verification**:
  - Ran `lab/run_matrix.sh` and verified `swanctl --list-sas` shows `ESTABLISHED` states on both `ns-left` and `ns-right` for all 4 profiles.
  - Ran `pytest -v tests/` with 7/7 tests passing.

---

## 3. What Is In Progress

- Phase 0 complete. Awaiting Phase 1 initiation.

---

## 4. What Is Broken / Blocking

- None. All test suites and lab runs are 100% green.

---

## 5. What to Avoid (Negative Constraints)

1. **DO NOT copy code** from public repositories (including CipherLens, IPsec Sentinel, Alchemist, CipherGuard, ipsec-Detector, SIH-160, vpnguard, VPN-Prots, IPsecAnalyzer, TunnelScope). All architecture and code must be 100% original.
2. **DO NOT omit provenance tags**. Every fact reported by the system must carry one of:
   - `observed` (from an active scan)
   - `parsed` (from an uploaded configuration)
   - `inferred` (from ML, with an explicit confidence score $\in [0.0, 1.0]$)
   - `unknown`
   *Rule evaluation constraint*: Any rule that requires an `unknown` fact MUST report `"cannot assess"`, NEVER a pass.
3. **DO NOT perform unconsented or intrusive active scanning**. Active scanning must strictly require:
   - Host present in an explicit allowlist
   - `consent` flag explicitly set to `True`
   - Non-intrusive proposal probes ONLY (no PSK dictionary attacks, no aggressive-mode credential harvesting).
4. **DO NOT use Docker or Containerlab for the Phase 0 lab testbed**. Must use native Linux network namespaces (`ip netns`) with separate `charon` instances and dedicated `swanctl` control sockets/directories.
5. **DO NOT skip ahead** to later phases before current phase exit criteria are fully satisfied and logged with concrete verification proof.

---

## 6. Phase Roadmap Status

| Phase | Description | Exit Criteria | Status |
|---|---|---|---|
| **Phase 0** | Repo & Testbed Foundation | All 4 swanctl profiles establish tunnel across namespaces; verified by `swanctl --list-sas` on both sides with logs | **PASSED (4/4)** |
| **Phase 1** | Passive Config Parser & Normalizer | Parse Cisco IOS, strongSwan, FortiOS; output normalized JSON schema with `parsed` provenance | PENDING |
| **Phase 2** | Active IKE Prober (Consent-Gated) | Synthesize IKEv1/v2 SA proposals, probe endpoint in allowlist, map responses with `observed` provenance | PENDING |
| **Phase 3** | Compliance & Vulnerability Engine | NIST SP 800-77r1, ANSSI, RFC 9395 rules; output `observed`/`parsed`/`unknown` tags; "cannot assess" on unknown | PENDING |
| **Phase 4** | Automated Remediation & Diff Generator | Generate hardened configuration files and unified diffs; round-trip validation with parser | PENDING |
| **Phase 5** | Passive PCAP/Live Capture Analyzer | Parse live IKE/ESP packets, detect SPI mismatches, unencrypted payloads, weak DH exchange | PENDING |
| **Phase 6** | ML Inference Engine & Confidence Tagging | Infer missing params with confidence scores $\in [0.0, 1.0]$; tag facts as `inferred` | PENDING |
| **Phase 7** | Cryptographic Seal & Integrity Verification | SHA-256 Merkle audit trail for every finding; cryptographic verification receipt | PENDING |
| **Phase 8** | API, CLI, and Web Dashboard | Fast backend API, CLI command runner, dynamic dark-mode UI with visual topology | PENDING |
| **Phase 9** | End-to-End Evaluation & Demonstration | Full automated test suite across all 4 lab profiles, compliance validation, remediation diffs, zero errors | PENDING |
