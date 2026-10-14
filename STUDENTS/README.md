# Students — SEMANTIC_KERNEL

**Project:** SEMANTIC_KERNEL  
**Category:** FRONTIER_HARNESSES  
**Upstream:** see BENCH.json  
**Pinned commit:** `cc8a15fa356f02dcb7bc64999392ca02f3167312`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `7d3df67f5db816b1f869e54fa9b7abd903cb06507ed66cbc4318f5800641fee4`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `cc8a15fa356f02dcb7bc64999392ca02f3167312`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `7d3df67f5db816b1f869e54fa9b7abd903cb06507ed66cbc4318f5800641fee4`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
