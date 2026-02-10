# muri-artifacts

Generated Groth16 artifacts for MuriData's proof-of-integrity system.

These files are produced by [`muri-zkproof`](https://github.com/MuriData/muri-zkproof) via `go run compile.go` and consumed by:

- **muri-contracts** — imports `poi_verifier.sol` as a Foundry submodule.
- **muri-node** / prover infrastructure — uses `poi_prover.key` and `poi_verifier.key` for proof generation and off-chain verification.

## Files

| File | Size | Purpose | Storage |
|------|------|---------|---------|
| `poi_verifier.sol` | ~26 KB | Solidity Groth16 verifier contract | Git |
| `poi_verifier.key` | ~492 B | Verifying key (public) | Git LFS |
| `poi_prover.key` | ~65 MB | Proving key (keep private in production) | Git LFS |

## Regenerating

From the `muri-zkproof` repository:

```bash
go run compile.go
```

Then copy the three output files here and commit. Downstream consumers update by bumping their submodule pin.

## Note

`poi_prover.key` is included here for development convenience. In production, distribute the proving key securely to prover infrastructure only — do not expose it publicly.
