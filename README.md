# TIBET / ZTIP Conformance — interop kits

**Runnable, vendor-free interop conformance for the TIBET / ZTIP substrate.**
Clone a kit, run it, watch it go green — then implement your own verifier against the vectors.
**The vectors are the contract; our `ref/` is just one implementation.**

The kits live in the canonical standards org **[Jtel-ZTIP-w3c](https://github.com/Jtel-ZTIP-w3c)**.
This repo is the ecosystem index — it does not duplicate them.

## The four families

| Family | Question it answers | Canonical repo |
|---|---|---|
| **ztip-conformance** | Can two implementations agree on actor proof, attestation, freshness, and `jis:`-namespace boundaries? *(v1: VINK attestation)* | [`Jtel-ZTIP-w3c/ztip-conformance`](https://github.com/Jtel-ZTIP-w3c/ztip-conformance) |
| **tibet-comms-conformance** | Can they resolve, reach, route, deliver, reject, and prove a message? *(ping · AINS sendpath · mux lane · overlay-route identity · I-Poll · Cmail · gateway egress · null-route)* | [`Jtel-ZTIP-w3c/tibet-comms-conformance`](https://github.com/Jtel-ZTIP-w3c/tibet-comms-conformance) |
| **tibet-evidence-conformance** | Can they store, seal, trace, restore, and report evidence? *(owns TimeVector causal order + UPIP reproducibility)* | [`Jtel-ZTIP-w3c/tibet-evidence-conformance`](https://github.com/Jtel-ZTIP-w3c/tibet-evidence-conformance) |
| **tibet-security-conformance** | Can they make the same allow/deny/quarantine/null-route decision and **fail closed**? | [`Jtel-ZTIP-w3c/tibet-security-conformance`](https://github.com/Jtel-ZTIP-w3c/tibet-security-conformance) |

> No fifth runtime family: the trust-kernel / OSAPI floor is exercised *through* the four above.

## Run any kit

```sh
git clone https://github.com/Jtel-ZTIP-w3c/ztip-conformance
cd ztip-conformance && ./run.sh        # no third-party deps; ends green or fails closed
```

## Bring your own verifier

Green `run.sh` proves the vectors are internally consistent — **that is not interop**.
Interop is when *your* independent implementation agrees with `vectors/*.json`. That is the point.

- **Hub / docs:** https://jtel-ztip-w3c.github.io
- **Demo app:** [`Jtel-ZTIP-w3c/ID-Drop`](https://github.com/Jtel-ZTIP-w3c/ID-Drop) — two phones tap, one offers, one verifies (APK in Releases)
- **License:** Apache-2.0 on every kit · open standard, open vectors
- **Conformance levels:** currently *structural* (internal consistency); *signed real-byte* vectors (Ed25519) are the next level for cross-vendor cryptographic proof.

*Open foundation. Custom applications on top are built with the customer — info@humotica.com*
