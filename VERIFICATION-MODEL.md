# Verification model — how each conformance vector is checked, and why

**The rule for every builder on the TIBET / ZTIP primitives.** It does not change. If you add a
vector, pick its class below and verify it that way — and ship the negative case.

> **Verification matches the primitive's job.** Don't sign what is deterministic; don't fake-sign
> what should be cryptographic. A signature on a policy decision adds nothing to interop; a
> structural check on an identity claim proves nothing. Each class has exactly one right answer.

## The three classes

| Class | Primitives (examples) | How it is verified | Negative case it ships |
|---|---|---|---|
| **Identity / actor-proof** | ztip: `vink`, `offer`, `resolve`, `challenge`, `entity`, `did-namespace` · comms: `ipoll`, `sealed`, `sendpath` | **Real Ed25519.** Sign a documented canonical string under a deterministic seed; the verifier checks the signature against the published public key. | `bad-signature` — a real, well-formed signature over a *tampered* canonical → must fail |
| **Evidence / integrity** | evidence: `content_hash`, `sealed_object`, `cbom_chain`, `wayback` | **Real SHA-256.** Re-hash the bytes and compare; chains link child `input_hash` to parent `content_hash`. | `tampered-byte` — one byte changed → hash mismatch → must fail |
| **Policy / logic / manifest** | security: `intent_policy`, `capability_gate`, `freshness`, `snaft_rule`, `cortex_consent`, `airlock_posture`, `null_route`, `verdict_event`, `no_fail_open` · evidence: `continuity`, `sbom`, `ai_sbom`, `trail_query`, `report_pack` | **Structural & deterministic.** Re-run the decision rule (or the completeness / ordering check) over the inputs; the allow/deny/quarantine/re-attest verdict — or the valid/invalid manifest result — is a pure function of the record. | the failing/edge case (`no-fail-open`, `stale-no-reattest`, `missing-purl`, `empty-trail`, …) → must fail / fail closed |

## Two rules that make it real, not theatre

1. **Every signed or hashed vector ships a negative case.** A passing `run.sh` only proves the
   vectors are internally consistent. The `bad-signature` / `tampered-byte` case is what proves the
   verification is *cryptographic* — a string-equality verifier would wrongly pass it. Tamper one
   byte of a real proof and that case must flip to fail.

2. **Structural is not weak — for logic it is the complete proof.** A policy decision is a
   deterministic function of its inputs; an independent implementation that computes the same
   verdict *is* the interop guarantee. Adding a signature there proves nothing about interop (the
   runtime signs its verdicts at deploy time — a separate, operational concern). Do not confuse a
   *logic* primitive that is correctly structural with an *identity/integrity* primitive that is
   missing its crypto.

## Determinism (so a second implementation matches byte-for-byte)

- Ed25519 is deterministic (RFC 8032): same seed + same canonical → byte-identical signature.
  Seeds are derived from a fixed namespace string per kit and are published in the vector.
- The **canonical string is the contract.** Sign and verify over exactly that string, nothing else.
- SHA-256 is canonical by construction. Hash the exact bytes named in the vector.

## For implementers

Bring your own verifier — do **not** import a kit's `ref/`. Read `vectors/*.json`, implement the
verification for each class above, and match the expected outcomes including the negative cases.
That, and only that, is conformance. The vectors are the contract; this model is how you read them.

*Apache-2.0 · part of the TIBET / ZTIP conformance family · canonical kits: github.com/Jtel-ZTIP-w3c*
