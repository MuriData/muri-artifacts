# muri-artifacts

Generated Groth16 artifacts for MuriData's zero-knowledge proof system.

These files are produced by [`muri-zkproof`](https://github.com/MuriData/muri-zkproof) and consumed by:

- **muri-contracts** — imports verifier contracts (e.g. `poi/poi_verifier.sol`) as a Foundry submodule.
- **muri-node** / prover infrastructure — uses proving and verifying keys for proof generation and off-chain verification.

## Structure

Artifacts are organized per-circuit:

```
muri-artifacts/
├── poi/
│   ├── poi_verifier.sol      # Solidity Groth16 verifier contract (~26 KB)
│   ├── poi_verifier.key      # Verifying key, public (~492 B, Git LFS)
│   └── poi_prover.key        # Proving key, keep private in prod (~65 MB, Git LFS)
└── .gitattributes            # *.key filter=lfs
```

## Regenerating

From the `muri-zkproof` repository:

```bash
# Dev mode (single-party, NOT for production)
go run ./cmd/compile poi dev

# Production (MPC ceremony)
go run ./cmd/compile poi ceremony ...
```

Then copy the three output files into the appropriate circuit subdirectory (e.g. `poi/`) and commit. Downstream consumers update by bumping their submodule pin:

```bash
cd muri-contracts
git submodule update --remote lib/muri-artifacts
```

## Adding a new circuit

1. Generate artifacts in `muri-zkproof` for the new circuit.
2. Create a new subdirectory here (e.g. `keyknowledge/`).
3. Copy the `*_verifier.sol`, `*_prover.key`, and `*_verifier.key` files into it.
4. Commit and push — key files are automatically tracked by Git LFS via `.gitattributes`.

## Note

Proving keys are included here for development convenience. In production, distribute proving keys securely to prover infrastructure only — do not expose them publicly.
